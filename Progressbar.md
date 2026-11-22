```dataviewjs
const page = dv.current();

const used = Number(page.token_used ?? 0);
const limit = Number(page.token_limit ?? 2000000);
const percent = Math.min((used / limit) * 100, 100);

const formatNumber = n =>
    n >= 1000000
        ? (n / 1000000).toFixed(2) + "M"
        : n >= 1000
            ? (n / 1000).toFixed(2) + "K"
            : n.toString();

const root = dv.el("div", "", {
    cls: "token-dashboard"
});

root.innerHTML = `
    <div class="token-header">
        <div class="token-heading">
            <span class="token-dot"></span>
            TOKEN BURN
            <span class="token-separator">•</span>
            ${page.window ?? "5H WINDOW"}
            <span class="token-separator">•</span>
            <span class="token-live">${page.status ?? "LIVE"}</span>
        </div>
        <span class="token-pull">LAST PULL: JUST NOW</span>
    </div>

    <div class="token-main">
        <div class="token-percent">
            ${Math.floor(percent)}<span>%</span>
        </div>

        <div class="token-track-area">
            <div class="token-track">
                <div class="token-fill"
                     style="width: ${percent}%">
                </div>
            </div>

            <div class="token-ticks">
                <span>0</span>
                <span>${formatNumber(limit * 0.25)}</span>
                <span>${formatNumber(limit * 0.5)}</span>
                <span>${formatNumber(limit * 0.75)}</span>
                <span>${formatNumber(limit)}</span>
            </div>
        </div>

        <div class="token-value">
            <strong>${formatNumber(used)}</strong>
            <span>/ ${formatNumber(limit)}</span>
        </div>
    </div>
`;
```

```dataviewjs

// ==========================================
// STUDY KPI DASHBOARD
// TERMINAL × CYBERPUNK MINIMAL
// ==========================================

// ---------- CONFIGURATION ----------

const SESSION_FOLDER =
    '"02_Areas/Work Sessions"';

// ---------- DATE RANGE: CURRENT WEEK ----------

const now = new Date();

const today = new Date(
    now.getFullYear(),
    now.getMonth(),
    now.getDate()
);

const weekStart = new Date(today);

weekStart.setDate(
    today.getDate() - ((today.getDay() + 6) % 7)
);

function toDateKey(date) {
    const y = date.getFullYear();
    const m = String(date.getMonth() + 1).padStart(2, "0");
    const d = String(date.getDate()).padStart(2, "0");

    return `${y}-${m}-${d}`;
}

const startKey = toDateKey(weekStart);
const todayKey = toDateKey(today);

// ---------- LOAD SESSION DATA ----------

const sessions = dv.pages(SESSION_FOLDER)
    .where(p => p.type === "work_session")
    .array()
    .map(p => {

        const date = dv.date(p.date);

        if (!date || !date.isValid) return null;

        const dateKey = date.toFormat("yyyy-MM-dd");

        // Only include sessions in current week
        if (
            dateKey < startKey ||
            dateKey > todayKey
        ) return null;

        // Checkbox completion
        const tasks = p.file.tasks ?? [];

        const completed = tasks.some(
            task => task.completed
        );

        if (!completed) return null;

        // Parse time
        const start = String(p.start ?? "");
        const end = String(p.end ?? "");

        const startMatch =
            start.match(/^(\d{2}):(\d{2})$/);

        const endMatch =
            end.match(/^(\d{2}):(\d{2})$/);

        if (!startMatch || !endMatch) return null;

        const startMinutes =
            Number(startMatch[1]) * 60 +
            Number(startMatch[2]);

        const endMinutes =
            Number(endMatch[1]) * 60 +
            Number(endMatch[2]);

        let duration = endMinutes - startMinutes;

        // Support overnight sessions
        if (duration <= 0) {
            duration += 1440;
        }

        return {
            name: p.file.name,
            category: String(p.category ?? "")
                .toLowerCase()
                .trim(),
            duration: duration / 60,
            date: dateKey
        };
    })
    .filter(Boolean);

// ---------- CALCULATE KPI ----------

const deepWorkSessions = sessions.filter(
    s => s.category === "deep_work"
);

const experimentSessions = sessions.filter(
    s => s.category === "experiment"
);

const researchSessions = sessions.filter(
    s => s.category === "research_review"
);

const totalDeepWorkHours = deepWorkSessions.reduce(
    (total, session) => total + session.duration,
    0
);

// ---------- KPI CONFIGURATION ----------

const kpis = [
    {
        title: "DEEP WORK",
        value: totalDeepWorkHours.toFixed(1),
        unit: "h",
        footerLeft: "THIS WEEK",
        footerRight: "HOURS LOGGED",
        color: "orange"
    },
    {
        title: "FOCUS SESSIONS",
        value: deepWorkSessions.length,
        unit: "",
        footerLeft: "THIS WEEK",
        footerRight: "SESSIONS COMPLETED",
        color: "green"
    },
    {
        title: "EXPERIMENTS",
        value: experimentSessions.length,
        unit: "",
        footerLeft: "THIS WEEK",
        footerRight: "PRACTICAL EXPERIMENTS",
        color: "purple"
    },
    {
        title: "RESEARCH REVIEWS",
        value: researchSessions.length,
        unit: "",
        footerLeft: "THIS WEEK",
        footerRight: "PAPERS / DOCUMENTS",
        color: "yellow"
    }
];

// ---------- DOM HELPER ----------

function el(tag, className, text = "") {
    const node = document.createElement(tag);

    if (className) {
        node.className = className;
    }

    if (text !== "") {
        node.textContent = text;
    }

    return node;
}

// ---------- RENDER ----------

const root = dv.container;

root.innerHTML = "";

const grid = el("div", "study-kpi-grid");

kpis.forEach(kpi => {

    const card = el(
        "section",
        `study-kpi-card ${kpi.color}`
    );

    // Header
    const header = el(
        "div",
        "study-kpi-header"
    );

    const title = el(
        "span",
        "study-kpi-title",
        kpi.title
    );

    const status = el(
        "span",
        "study-kpi-status"
    );

    header.append(title, status);

    // Main value
    const value = el(
        "div",
        "study-kpi-value"
    );

    value.appendChild(
        document.createTextNode(String(kpi.value))
    );

    if (kpi.unit) {
        value.appendChild(
            el("small", "", kpi.unit)
        );
    }

    // Footer
    const footer = el(
        "div",
        "study-kpi-footer"
    );

    footer.append(
        el("span", "", kpi.footerLeft),
        el("span", "", kpi.footerRight)
    );

    // Decorative accent line
    const track = el(
        "div",
        "study-kpi-track"
    );

    const fill = el("div", "");

    track.appendChild(fill);

    // Assemble card
    card.append(
        header,
        value,
        footer,
        track
    );

    grid.appendChild(card);
});

// ---------- EMPTY / ERROR HANDLING ----------

if (kpis.length === 0) {
    root.appendChild(
        el("p", "workspace-empty", "NO KPI DATA")
    );
} else {
    root.appendChild(grid);
}

```

```dataviewjs
const CONFIG = {
    dailyFolder: "01 Daily Notes",
    ideaFolder: "00 Inbox/Ideas",
    learningFolder: "02 Learning",
    projectOverview: "03 Projects/Projects Overview.md",
    inboxNote: "00 Inbox/Inbox.md",
    weeklyFolder: "06 Reviews/Weekly",
    cleanupNote: "06 Reviews/Vault Cleanup.md",
    sessionFolder: "05 Tasks/Work Sessions",
};

// ---------- UTILITIES ----------

const container = dv.container;
container.innerHTML = "";

// Khung bọc + lưới chứa các nút (CSS dùng 2 class này để chia cột)
const wrap = document.createElement("div");
wrap.className = "cc-action-wrap";

const grid = document.createElement("div");
grid.className = "cc-action-grid";

wrap.appendChild(grid);
container.appendChild(wrap);

const moment = window.moment;
const today = moment().format("YYYY-MM-DD");
const week = moment().format("GGGG-[W]WW");
const now = moment();

async function ensureFolder(path) {
    const parts = path.split("/");
    let current = "";

    for (const part of parts) {
        current = current ? `${current}/${part}` : part;

        if (!app.vault.getAbstractFileByPath(current)) {
            try {
                await app.vault.createFolder(current);
            } catch (e) {}
        }
    }
}

async function openOrCreate(path, content = "") {
    await ensureFolder(path.substring(0, path.lastIndexOf("/")));

    let file = app.vault.getAbstractFileByPath(path);

    if (!file) {
        file = await app.vault.create(path, content);
    }

    const leaf = app.workspace.getLeaf(false);
    await leaf.openFile(file);
}

function createButton(icon, title, description, action) {
    const button = document.createElement("button");
    button.className = "cc-action-button";

    button.innerHTML = `
        <span class="cc-action-icon">${icon}</span>
        <span class="cc-action-title">${title}</span>
        <span class="cc-action-desc">${description}</span>
        <span class="cc-action-arrow">↗</span>
    `;

    button.addEventListener("click", async () => {
        button.classList.add("is-loading");

        try {
            await action();
        } catch (error) {
            console.error(error);
            new Notice("Action failed: " + error.message);
        } finally {
            button.classList.remove("is-loading");
        }
    });

    grid.appendChild(button);
}

// ---------- ACTION 1: PLAN TODAY ----------

createButton(
    "◷",
    "PLAN TODAY",
    "Create daily plan",
    async () => {
        const path = `${CONFIG.dailyFolder}/${today}.md`;

        const template = `---
type: daily
date: ${today}
status: planned
---

# Daily Plan

## Objectives
- [ ]

## Tasks
- [ ]

## Schedule

## Reflection

### Completed

### Not Completed

### Notes
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 2: CAPTURE IDEA ----------

createButton(
    "✳",
    "CAPTURE IDEA",
    "Save a new idea",
    async () => {
        const timestamp = moment().format("HH-mm-ss");
        const path = `${CONFIG.ideaFolder}/${today}-${timestamp}.md`;

        const template = `---
type: idea
created: ${now.format("YYYY-MM-DD HH:mm")}
status: inbox
---

# New Idea

## Description

## Related Projects

## Notes
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 3: START LEARNING ----------

createButton(
    "⌘",
    "START LEARNING",
    "Open active lesson",
    async () => {
        const lessons = dv.pages(`"${CONFIG.learningFolder}"`)
            .where(p =>
                p.status === "learning" &&
                /^Lesson-\d+$/i.test(p.file.name)
            )
            .array()
            .sort((a, b) =>
                Number(a.lesson_number ?? 0) -
                Number(b.lesson_number ?? 0)
            );

        if (!lessons.length) {
            new Notice("No lesson has status: learning.");
            return;
        }

        if (lessons.length > 1) {
            new Notice("Multiple active lessons found. Opening the first one.");
        }

        const file = app.vault.getAbstractFileByPath(
            lessons[0].file.path
        );

        await app.workspace.getLeaf(false).openFile(file);
    }
);

// ---------- ACTION 4: PROJECT REVIEW ----------

createButton(
    "▤",
    "PROJECT REVIEW",
    "Review active projects",
    async () => {
        const template = `---
type: project-dashboard
---

# Project Overview

## Active Projects

## Progress

## Blockers

## Next Actions
`;

        await openOrCreate(CONFIG.projectOverview, template);
    }
);

// ---------- ACTION 5: WEEKLY REVIEW ----------

createButton(
    "▥",
    "WEEKLY REVIEW",
    "Review the past 7 days",
    async () => {
        const path = `${CONFIG.weeklyFolder}/${week}.md`;

        const start = moment().startOf("isoWeek").format("YYYY-MM-DD");
        const end = moment().endOf("isoWeek").format("YYYY-MM-DD");

        const template = `---
type: weekly-review
week: ${week}
start_date: ${start}
end_date: ${end}
status: reviewing
---

# Weekly Review

## 1. Statistics

### Deep Work

### Completed Tasks

### Learning Sessions

## 2. Achievements

## 3. Incomplete Tasks

## 4. Reflection

### What went well?

### What went wrong?

### What distracted me?

## 5. Next Week

### Priorities

- [ ]

### Improvements
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 6: INBOX PROCESSING ----------

createButton(
    "▣",
    "INBOX PROCESSING",
    "Process unorganized notes",
    async () => {
        const template = `---
type: inbox
---

# Inbox

## Unprocessed Notes

## To Organize
`;

        await openOrCreate(CONFIG.inboxNote, template);
    }
);

// ---------- ACTION 7: DEEP WORK ----------

createButton(
    "◉",
    "DEEP WORK",
    "Start / stop focus session",
    async () => {
        await ensureFolder(CONFIG.sessionFolder);

        const activePath =
            `${CONFIG.sessionFolder}/Active Session.md`;

        let file = app.vault.getAbstractFileByPath(activePath);

        // Stop current session
        if (file) {
            const fm = app.metadataCache.getFileCache(file)?.frontmatter;

            if (fm?.status === "active") {
                const startTime = moment(fm.start_time);
                const endTime = moment();
                const duration = Math.max(
                    0,
                    Math.round(endTime.diff(startTime, "seconds") / 60)
                );

                await app.fileManager.processFrontMatter(
                    file,
                    frontmatter => {
                        frontmatter.status = "completed";
                        frontmatter.end_time = endTime.format();
                        frontmatter.duration_minutes = duration;
                    }
                );

                const archivePath =
                    `${CONFIG.sessionFolder}/${today}-${startTime.format("HH-mm-ss")}.md`;

                await app.vault.rename(file, archivePath);

                new Notice(`Deep Work completed: ${duration} minutes.`);
                return;
            }
        }

        // Start new session
        const startTime = moment();

        const template = `---
type: work-session
date: ${today}
status: active
focus: deep-work
start_time: ${startTime.format()}
duration_minutes: 0
---

# Deep Work Session

## Objective

## Notes
`;

        await app.vault.create(activePath, template);
        new Notice("Deep Work session started.");
    }
);

// ---------- ACTION 8: VAULT CLEANUP ----------

createButton(
    "⌁",
    "VAULT CLEANUP",
    "Maintain your workspace",
    async () => {
        const template = `---
type: maintenance
---

# Vault Cleanup

## Inbox
- [ ] Process unorganized notes

## Projects
- [ ] Check project properties
- [ ] Archive completed projects

## Learning
- [ ] Check lesson status
- [ ] Update learning progress

## General
- [ ] Check broken links
- [ ] Remove unused attachments
- [ ] Review tags and properties
`;

        await openOrCreate(CONFIG.cleanupNote, template);
    }
);
```

```dataviewjs

(async () => {

// ============================================
// SCHEDULE + DAILY TASKS
// DATA SOURCE: MARKDOWN FILES IN OBSIDIAN VAULT
// ============================================


// ============================================
// 1. CONFIGURATION
// ============================================

const SCHEDULE_PATH = "02_Areas/Daily Schedule.md";
const DAILY_TASK_PATH = "02_Areas/Daily Tasks.md";

const root = dv.container;
root.innerHTML = "";


// ============================================
// 2. DOM HELPERS
// ============================================

function el(tag, className, text = "") {
    const node = document.createElement(tag);

    if (className) node.className = className;
    if (text !== "") node.textContent = text;

    return node;
}


// ============================================
// 3. READ SCHEDULE MARKDOWN
// ============================================

const scheduleFile =
    app.vault.getAbstractFileByPath(SCHEDULE_PATH);

let scheduleEvents = [];
let scheduleError = null;

if (scheduleFile) {
    try {
        const content = await app.vault.read(scheduleFile);

        scheduleEvents = content
            .split("\n")
            .map(line => {

                // Hỗ trợ:
                // - 08:00 | Gym
                // * 09:00 | Study
                // 10:00 | Meeting

                const match = line.match(
                    /^\s*(?:[-*+]\s*)?(\d{2}:\d{2})\s*\|\s*(.+?)\s*$/
                );

                if (!match) return null;

                return {
                    time: match[1],
                    title: match[2]
                };
            })
            .filter(Boolean)
            .sort((a, b) =>
                a.time.localeCompare(b.time)
            );

    } catch (error) {
        console.error("Could not read schedule:", error);
        scheduleError = error;
    }
}


// ============================================
// 4. READ DAILY TASKS
// ============================================

const dailyPage = dv.page("02_Areas/Daily Tasks");

function getTasks(page) {
    if (!page) return [];

    return (page.file.tasks ?? []).map(task => ({
        ...task,
        path: page.file.path,
        completed: Boolean(task.completed)
    }));
}

const dailyTasks = getTasks(dailyPage);


// ============================================
// 5. MAIN LAYOUT
// ============================================

const workspace = el("div", "workspace-grid");

root.appendChild(workspace);


// ============================================
// 6. PROGRESS FUNCTION
// ============================================

function updateProgress(tasks, fill, count, percent) {

    const total = tasks.length;

    const completed = tasks.filter(
        task => task.completed
    ).length;

    const ratio = total
        ? Math.round(completed / total * 100)
        : 0;

    fill.style.width = `${ratio}%`;

    count.textContent = `${completed}/${total}`;

    percent.textContent = `${ratio}%`;
}


// ============================================
// 7. TASK RENDERER
// ============================================

function renderTask(task, container, onUpdate) {

    const row = el("label", "workspace-task");

    const checkbox = el("input", "workspace-checkbox");

    checkbox.type = "checkbox";
    checkbox.checked = task.completed;

    const text = el(
        "span",
        "workspace-task-text",
        task.text
    );

    if (task.completed) {
        text.classList.add("is-completed");
    }


    // Checkbox interaction

    checkbox.addEventListener("change", async () => {

        const newState = checkbox.checked;

        const file = app.vault.getAbstractFileByPath(
            task.path
        );

        if (!file) {
            checkbox.checked = task.completed;
            return;
        }

        checkbox.disabled = true;

        try {

            await app.vault.process(file, content => {

                const lines = content.split("\n");

                // Dataview line number thường bắt đầu từ 1.
                // Thử cả hai vị trí để tương thích.

                const candidates = [
                    Number(task.line) - 1,
                    Number(task.line)
                ].filter((value, index, array) =>
                    Number.isInteger(value) &&
                    value >= 0 &&
                    value < lines.length &&
                    array.indexOf(value) === index
                );

                const checkboxRegex =
                    /^(\s*(?:[-*+]|\d+\.)\s*)\[([ xX])\](.*)$/;

                // Ưu tiên dòng khớp nội dung task
                let lineIndex = candidates.find(index => {
                    const line = lines[index];

                    return checkboxRegex.test(line) &&
                        line.includes(task.text);
                });

                // Nếu không khớp nội dung, thử vị trí task
                if (lineIndex === undefined) {
                    lineIndex = candidates.find(index =>
                        checkboxRegex.test(lines[index])
                    );
                }

                if (lineIndex === undefined) {
                    console.warn(
                        "Could not locate task line:",
                        task.text
                    );

                    return content;
                }

                const line = lines[lineIndex];

                lines[lineIndex] = line.replace(
                    checkboxRegex,
                    (_, prefix, oldState, rest) =>
                        `${prefix}[${newState ? "x" : " "}]${rest}`
                );

                return lines.join("\n");
            });

            task.completed = newState;

            text.classList.toggle(
                "is-completed",
                newState
            );

            onUpdate();

        } catch (error) {

            console.error(
                "Could not update task:",
                error
            );

            checkbox.checked = task.completed;

        } finally {

            checkbox.disabled = false;

        }
    });


    row.append(checkbox, text);

    container.appendChild(row);
}


// ============================================
// 8. CARD HEADER
// ============================================

function createCardHeader(title, iconName, countText) {

    const header = el("div", "workspace-card-header");

    const heading = el("div", "workspace-card-title");

    heading.append(
        el("span", "workspace-card-icon", iconName),
        el("span", "", title)
    );

    const count = el(
        "span",
        "workspace-count",
        countText
    );

    header.append(heading, count);

    return {
        header,
        count
    };
}


// ============================================
// 9. SCHEDULE PANEL
// ============================================

function createScheduleCard() {

    const card = el(
        "section",
        "workspace-card schedule-card"
    );

    const { header } = createCardHeader(
        "SCHEDULE",
        "◷",
        `${scheduleEvents.length} EVENTS`
    );

    const grid = el("div", "schedule-grid");

    const leftColumn = el("div", "schedule-column");

    const rightColumn = el("div", "schedule-column");


    // Split events into two columns

    const midpoint = Math.ceil(
        scheduleEvents.length / 2
    );

    scheduleEvents.forEach((event, index) => {

        const row = el("div", "schedule-event");

        const time = el(
            "span",
            "schedule-time",
            event.time
        );

        const title = el(
            "span",
            "schedule-title",
            event.title
        );

        row.append(time, title);

        if (index < midpoint) {
            leftColumn.appendChild(row);
        } else {
            rightColumn.appendChild(row);
        }
    });


    // Empty state

    if (scheduleEvents.length === 0) {

        let message = "No schedule events found.";

        if (!scheduleFile) {
            message = "Daily Schedule.md not found.";
        } else if (scheduleError) {
            message = "Could not read schedule file.";
        }

        grid.appendChild(
            el("p", "workspace-empty", message)
        );

    } else {

        grid.append(leftColumn, rightColumn);

    }


    card.append(header, grid);

    return card;
}


// ============================================
// 10. DAILY TASKS PANEL
// ============================================

function createDailyCard() {

    const card = el(
        "section",
        "workspace-card daily-card"
    );


    // Header

    const { header, count } = createCardHeader(
        "DAILY TASKS",
        "▣",
        "0/0"
    );


    // Progress information

    const stats = el("div", "workspace-stats");

    const percent = el(
        "span",
        "workspace-percent",
        "0%"
    );

    const caption = el(
        "span",
        "workspace-caption",
        "DAILY COMPLETION"
    );

    stats.append(percent, caption);

    // Task grid

    const taskGrid = el(
        "div",
        "workspace-task-grid"
    );

    dailyTasks.forEach(task => {

        renderTask(task, taskGrid, () => {

            updateProgress(
                dailyTasks,
                fill,
                count,
                percent
            );

        });

    });


    // Empty state

    if (!dailyPage) {

        card.appendChild(
            el(
                "p",
                "workspace-empty",
                "Daily Tasks.md not found."
            )
        );

    } else if (dailyTasks.length === 0) {

        card.appendChild(
            el(
                "p",
                "workspace-empty",
                "No tasks available."
            )
        );

    }


    card.append(
        header,
        taskGrid
    );

    return card;
}


// ============================================
// 11. RENDER DASHBOARD
// ============================================

workspace.append(
    createScheduleCard(),
    createDailyCard()
);

})();

```
```dataviewjs
const lessons = dv.pages('"02_Areas/Course"')
    .where(p =>
        /^Lesson(-\d+)?.md$/i.test(p.file.name) &&
        p.status === "learning"
    )
    .array()
    .sort((a, b) =>
        Number(a.lesson_number ?? 0) -
        Number(b.lesson_number ?? 0)
    );

const root = dv.container;
root.innerHTML = "";

function create(tag, cls, text = "") {
    const node = document.createElement(tag);
    if (cls) node.className = cls;
    if (text !== "") node.textContent = text;
    return node;
}

// Course Status
const card = create("section", "course-status");
root.appendChild(card);

// Header
const header = create("div", "course-header");
const heading = create("div", "course-heading");

heading.append(
    create("span", "course-icon", "⌁"),
    create("span", "", "COURSE STATUS")
);

const badge = create(
    "span",
    "course-status-badge",
    lessons.length ? "LEARNING" : "NO ACTIVE LESSON"
);

if (!lessons.length) {
    badge.classList.add("course-inactive");
}

header.append(heading, badge);
card.appendChild(header);

// Current Lesson
const current = create("div", "current-lesson");

if (lessons.length > 0) {
    const page = lessons[0];

    const number = Number(page.lesson_number ?? 0);
    const total = Number(page.total_lessons ?? 0);

    const progress = total > 0
        ? Math.min(100, number / total * 100)
        : 0;

    const top = create("div", "course-meta");

    top.append(
        create("span", "course-label", "CURRENT LESSON"),
        create("span", "course-number",
            total
                ? `${number} / ${total} LESSONS`
                : `LESSON ${number || "—"}`
        )
    );

    const info = create("div", "course-info");

    const left = create("div", "course-info-left");

    left.append(
        create("div", "course-lesson-name", page.file.name),
        create("div", "course-name",
            `${page.course ?? "Computer Vision"} · ${page.platform ?? "Learning"}`
        )
    );

    const right = create("div", "course-info-right");

    right.append(
        create("div", "course-percentage", `${Math.round(progress)}%`),
        create("div", "course-caption", "COURSE PROGRESS")
    );

    info.append(left, right);

    const track = create("div", "course-progress-track");
    const fill = create("div", "course-progress-fill");

    fill.style.width = `${progress}%`;
    track.appendChild(fill);

    const bottom = create("div", "course-bottom");

    bottom.append(
        create("span", "course-duration",
            `${page.duration ?? 0} MINUTES`
        )
    );

    // Open Current Lesson
    const button = create(
        "button",
        "course-open",
        "OPEN CURRENT LESSON ↗"
    );

    button.addEventListener("click", async () => {
        await app.workspace.openLinkText(
            page.file.path,
            "",
            false
        );
    });

    bottom.appendChild(button);

    current.append(top, info, track, bottom);

} else {
    current.append(
        create("div", "course-label", "CURRENT LESSON"),
        create("div", "course-lesson-name", "No active lesson"),
        create("div", "course-name",
            "Select a lesson and set status to learning."
        )
    );
}

card.appendChild(current);
```
```dataviewjs
const projects = dv.pages('"01_Projects"')
    .where(p =>
        p.type === "project" &&
        p.status === "active" &&
        p.deadline
    )
    .array()
    .map(p => {
        const deadline = dv.date(p.deadline);
        const today = dv.date("today");

        const daysLeft = Math.ceil(
            deadline.startOf("day").diff(
                today.startOf("day"),
                "days"
            ).days
        );

        return {
            page: p,
            name: p.title ?? p.file.name,
            deadline,
            daysLeft,
            progress: Number(p.progress ?? 0),
            priority: p.priority ?? "normal"
        };
    })
    .sort((a, b) => a.daysLeft - b.daysLeft);


// DOM helper
function el(tag, cls, text = "") {
    const node = document.createElement(tag);
    if (cls) node.className = cls;
    if (text !== "") node.textContent = text;
    return node;
}


// Main panel
const root = dv.container;
root.innerHTML = "";

const card = el("section", "project-deadline-card");

const header = el("div", "project-deadline-header");

header.append(
    el("span", "project-deadline-title", "PROJECT DEADLINES"),
    el("span", "project-deadline-count",
        `${projects.length} ACTIVE`
    )
);

card.appendChild(header);


// Project list
projects.forEach(project => {

    const item = el("div", "deadline-project");

    const top = el("div", "deadline-project-top");

    const name = el(
        "span",
        "deadline-project-name",
        project.name
    );

    let statusText;
    let statusClass;

    if (project.daysLeft < 0) {
        statusText = `${Math.abs(project.daysLeft)} DAYS OVERDUE`;
        statusClass = "deadline-overdue";
    } else if (project.daysLeft === 0) {
        statusText = "DUE TODAY";
        statusClass = "deadline-today";
    } else {
        statusText = `${project.daysLeft} DAYS LEFT`;
        statusClass = project.daysLeft <= 7
            ? "deadline-warning"
            : "deadline-normal";
    }

    const badge = el(
        "span",
        `deadline-badge ${statusClass}`,
        statusText
    );

    top.append(name, badge);


    // Metadata
    const meta = el("div", "deadline-project-meta");

    const dateText = project.deadline.toFormat("dd MMM yyyy");

    meta.append(
        el("span", "", dateText),
        el("span", "", `${project.priority.toUpperCase()} PRIORITY`),
        el("span", "", `${project.progress}%`)
    );


    // Progress
    const track = el("div", "deadline-progress-track");
    const fill = el("div", "deadline-progress-fill");

    fill.style.width =
        `${Math.min(100, Math.max(0, project.progress))}%`;

    track.appendChild(fill);


    // Open project note
    const link = el(
        "button",
        "deadline-open",
        "OPEN PROJECT ↗"
    );

    link.addEventListener("click", async () => {
        await app.workspace.openLinkText(
            project.page.file.path,
            "",
            false
        );
    });

    item.append(top, meta, track, link);

    card.appendChild(item);
});


// Empty state
if (projects.length === 0) {
    card.appendChild(
        el("p", "workspace-empty", "No active projects found.")
    );
}

root.appendChild(card);
```

```dataviewjs
// DOM helper
function el(tag, cls, text = "") {
    const node = document.createElement(tag);
    if (cls) node.className = cls;
    if (text !== "") node.textContent = text;
    return node;
}

async function openBase(path) {
    const file = app.vault.getAbstractFileByPath(path);

    if (!file) {
        new Notice(`Base file not found: ${path}`);
        return;
    }

    await app.workspace.getLeaf(false).openFile(file);
}

const CONFIG = {
    dailyFolder: "01 Daily Notes",
    ideaFolder: "00 Inbox/Ideas",
    learningFolder: "02_Area/Learning",
    projectBase: "01_Projects/ProjectsOverview.base",
    inboxNote: "00 Inbox/Inbox.base",
    weeklyFolder: "06 Reviews/Weekly",
    cleanupNote: "06 Reviews/Vault Cleanup.base",
    sessionFolder: "05 Tasks/Work Sessions",
};

// ---------- UTILITIES ----------

const container = dv.container;
container.innerHTML = "";

// Khung bọc + lưới chứa các nút (CSS dùng 2 class này để chia cột)
const wrap = document.createElement("div");
wrap.className = "cc-action-wrap";

const grid = document.createElement("div");
grid.className = "cc-action-grid";

wrap.appendChild(grid);
container.appendChild(wrap);

const moment = window.moment;
const today = moment().format("YYYY-MM-DD");
const week = moment().format("GGGG-[W]WW");
const now = moment();

async function ensureFolder(path) {
    const parts = path.split("/");
    let current = "";

    for (const part of parts) {
        current = current ? `${current}/${part}` : part;

        if (!app.vault.getAbstractFileByPath(current)) {
            try {
                await app.vault.createFolder(current);
            } catch (e) {}
        }
    }
}

async function openOrCreate(path, content = "") {
    await ensureFolder(path.substring(0, path.lastIndexOf("/")));

    let file = app.vault.getAbstractFileByPath(path);

    if (!file) {
        file = await app.vault.create(path, content);
    }

    const leaf = app.workspace.getLeaf(false);
    await leaf.openFile(file);
}

function createButton(icon, title, description, action) {
    const button = document.createElement("button");
    button.className = "cc-action-button";

    button.innerHTML = `
        <span class="cc-action-icon">${icon}</span>
        <span class="cc-action-title">${title}</span>
        <span class="cc-action-desc">${description}</span>
        <span class="cc-action-arrow">↗</span>
    `;

    button.addEventListener("click", async () => {
        button.classList.add("is-loading");

        try {
            await action();
        } catch (error) {
            console.error(error);
            new Notice("Action failed: " + error.message);
        } finally {
            button.classList.remove("is-loading");
        }
    });

    grid.appendChild(button);
}

// ---------- ACTION 1: PLAN TODAY ----------

createButton(
    "◷",
    "PLAN TODAY",
    "Create daily plan",
    async () => {
        const path = `${CONFIG.dailyFolder}/${today}.md`;

        const template = `---
type: daily
date: ${today}
status: planned
---

# Daily Plan

## Objectives
- [ ]

## Tasks
- [ ]

## Schedule

## Reflection

### Completed

### Not Completed

### Notes
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 2: CAPTURE IDEA ----------

createButton(
    "✳",
    "CAPTURE IDEA",
    "Save a new idea",
    async () => {
        const timestamp = moment().format("HH-mm-ss");
        const path = `${CONFIG.ideaFolder}/${today}-${timestamp}.md`;

        const template = `---
type: idea
created: ${now.format("YYYY-MM-DD HH:mm")}
status: inbox
---

# New Idea

## Description

## Related Projects

## Notes
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 3: START LEARNING ----------

createButton(
    "⌘",
    "START LEARNING",
    "Open active lesson",
    async () => {
        const lessons = dv.pages(`"${CONFIG.learningFolder}"`)
            .where(p =>
                p.status === "learning" &&
                /^Lesson-\d+$/i.test(p.file.name)
            )
            .array()
            .sort((a, b) =>
                Number(a.lesson_number ?? 0) -
                Number(b.lesson_number ?? 0)
            );

        if (!lessons.length) {
            new Notice("No lesson has status: learning.");
            return;
        }

        if (lessons.length > 1) {
            new Notice("Multiple active lessons found. Opening the first one.");
        }

        const file = app.vault.getAbstractFileByPath(
            lessons[0].file.path
        );

        await app.workspace.getLeaf(false).openFile(file);
    }
);

// ---------- ACTION 4: PROJECT REVIEW ----------

createButton(
    "▤",
    "PROJECT REVIEW",
    "Review active projects",
    async () => {
        await openBase(CONFIG.projectBase);
    }
);


// ---------- ACTION 5: WEEKLY REVIEW ----------

createButton(
    "▥",
    "WEEKLY REVIEW",
    "Review the past 7 days",
    async () => {
        const path = `${CONFIG.weeklyFolder}/${week}.md`;

        const start = moment().startOf("isoWeek").format("YYYY-MM-DD");
        const end = moment().endOf("isoWeek").format("YYYY-MM-DD");

        const template = `---
type: weekly-review
week: ${week}
start_date: ${start}
end_date: ${end}
status: reviewing
---

# Weekly Review

## 1. Statistics

### Deep Work

### Completed Tasks

### Learning Sessions

## 2. Achievements

## 3. Incomplete Tasks

## 4. Reflection

### What went well?

### What went wrong?

### What distracted me?

## 5. Next Week

### Priorities

- [ ]

### Improvements
`;

        await openOrCreate(path, template);
    }
);

// ---------- ACTION 6: INBOX PROCESSING ----------

createButton(
    "▣",
    "INBOX PROCESSING",
    "Process unorganized notes",
    async () => {
        const template = `---
type: inbox
---

# Inbox

## Unprocessed Notes

## To Organize
`;

        await openOrCreate(CONFIG.inboxNote, template);
    }
);

// ---------- ACTION 7: DEEP WORK ----------

createButton(
    "◉",
    "DEEP WORK",
    "Start / stop focus session",
    async () => {
        await ensureFolder(CONFIG.sessionFolder);

        const activePath =
            `${CONFIG.sessionFolder}/Active Session.md`;

        let file = app.vault.getAbstractFileByPath(activePath);

        // Stop current session
        if (file) {
            const fm = app.metadataCache.getFileCache(file)?.frontmatter;

            if (fm?.status === "active") {
                const startTime = moment(fm.start_time);
                const endTime = moment();
                const duration = Math.max(
                    0,
                    Math.round(endTime.diff(startTime, "seconds") / 60)
                );

                await app.fileManager.processFrontMatter(
                    file,
                    frontmatter => {
                        frontmatter.status = "completed";
                        frontmatter.end_time = endTime.format();
                        frontmatter.duration_minutes = duration;
                    }
                );

                const archivePath =
                    `${CONFIG.sessionFolder}/${today}-${startTime.format("HH-mm-ss")}.md`;

                await app.vault.rename(file, archivePath);

                new Notice(`Deep Work completed: ${duration} minutes.`);
                return;
            }
        }

        // Start new session
        const startTime = moment();

        const template = `---
type: work-session
date: ${today}
status: active
focus: deep-work
start_time: ${startTime.format()}
duration_minutes: 0
---

# Deep Work Session

## Objective

## Notes
`;

        await app.vault.create(activePath, template);
        new Notice("Deep Work session started.");
    }
);

// ---------- ACTION 8: VAULT CLEANUP ----------

createButton(
    "⌁",
    "VAULT CLEANUP",
    "Maintain your workspace",
    async () => {
        const template = `---
type: maintenance
---

# Vault Cleanup

## Inbox
- [ ] Process unorganized notes

## Projects
- [ ] Check project properties
- [ ] Archive completed projects

## Learning
- [ ] Check lesson status
- [ ] Update learning progress

## General
- [ ] Check broken links
- [ ] Remove unused attachments
- [ ] Review tags and properties
`;

        await openOrCreate(CONFIG.cleanupNote, template);
    }
);
```

