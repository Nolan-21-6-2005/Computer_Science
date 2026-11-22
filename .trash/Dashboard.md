```dataviewjs
// ===== 1. TOKEN BURN =====
const page = dv.current();
const used = Number(page.token_used ?? 0);
const limit = Number(page.token_limit ?? 2000000);
const percent = Math.min((used / limit) * 100, 100);

const fmt = n => n >= 1e6 ? (n / 1e6).toFixed(2) + "M"
              : n >= 1e3 ? (n / 1e3).toFixed(2) + "K"
              : String(n);

const root = dv.container;
root.innerHTML = "";
const box = document.createElement("div");
box.className = "token-dashboard";
box.innerHTML = `
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
        <div class="token-percent">${Math.floor(percent)}<span>%</span></div>
        <div class="token-track-area">
            <div class="token-track"><div class="token-fill" style="width: ${percent}%"></div></div>
            <div class="token-ticks">
                <span>0</span>
                <span>${fmt(limit * 0.25)}</span>
                <span>${fmt(limit * 0.5)}</span>
                <span>${fmt(limit * 0.75)}</span>
                <span>${fmt(limit)}</span>
            </div>
        </div>
        <div class="token-value"><strong>${fmt(used)}</strong><span>/ ${fmt(limit)}</span></div>
    </div>`;
root.appendChild(box);
```

```dataviewjs
// ===== 2. STUDY KPI =====
const CFG = JSON.parse(await dv.io.load("99_System/config.json"));

const now = new Date();
const today = new Date(now.getFullYear(), now.getMonth(), now.getDate());
const weekStart = new Date(today);
weekStart.setDate(today.getDate() - ((today.getDay() + 6) % 7));

const key = d => `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(d.getDate()).padStart(2, "0")}`;
const startKey = key(weekStart);
const todayKey = key(today);

const toMin = s => {
    const m = String(s ?? "").match(/^(\d{1,2}):(\d{2})$/);
    return m ? Number(m[1]) * 60 + Number(m[2]) : null;
};

const sessions = dv.pages(`"${CFG.folders.sessions}"`)
    .where(p => p.type === CFG.types.session)
    .array()
    .map(p => {
        const date = dv.date(p.date);
        if (!date || !date.isValid) return null;
        const dateKey = date.toFormat("yyyy-MM-dd");
        if (dateKey < startKey || dateKey > todayKey) return null;

        // Hoàn thành = status completed HOẶC có checkbox đã tick
        const done = p.status === "completed" || (p.file.tasks ?? []).some(t => t.completed);
        if (!done) return null;

        // Thời lượng: ưu tiên duration_minutes, nếu không có thì tính từ start/end
        let minutes;
        if (p.duration_minutes !== undefined && p.duration_minutes !== null && p.duration_minutes !== "") {
            minutes = Number(p.duration_minutes);
        } else {
            const s = toMin(p.start), e = toMin(p.end);
            if (s === null || e === null) return null;
            minutes = e - s;
            if (minutes <= 0) minutes += 1440; // phiên qua đêm
        }

        return {
            category: String(p.category ?? "").toLowerCase().trim(),
            hours: minutes / 60
        };
    })
    .filter(Boolean);

const by = c => sessions.filter(s => s.category === c);
const deep = by("deep_work");
const deepHours = deep.reduce((t, s) => t + s.hours, 0);

const kpis = [
    { title: "DEEP WORK", value: deepHours.toFixed(1), unit: "h", l: "THIS WEEK", r: "HOURS LOGGED", color: "orange" },
    { title: "FOCUS SESSIONS", value: deep.length, unit: "", l: "THIS WEEK", r: "SESSIONS COMPLETED", color: "green" },
    { title: "EXPERIMENTS", value: by("experiment").length, unit: "", l: "THIS WEEK", r: "PRACTICAL EXPERIMENTS", color: "purple" },
    { title: "RESEARCH REVIEWS", value: by("research_review").length, unit: "", l: "THIS WEEK", r: "PAPERS / DOCUMENTS", color: "yellow" }
];

function el(tag, cls, text = "") {
    const n = document.createElement(tag);
    if (cls) n.className = cls;
    if (text !== "") n.textContent = text;
    return n;
}

const root = dv.container;
root.innerHTML = "";
const grid = el("div", "study-kpi-grid");

kpis.forEach(k => {
    const card = el("section", `study-kpi-card ${k.color}`);

    const header = el("div", "study-kpi-header");
    header.append(el("span", "study-kpi-title", k.title), el("span", "study-kpi-status"));

    const value = el("div", "study-kpi-value");
    value.appendChild(document.createTextNode(String(k.value)));
    if (k.unit) value.appendChild(el("small", "", k.unit));

    const footer = el("div", "study-kpi-footer");
    footer.append(el("span", "", k.l), el("span", "", k.r));

    const track = el("div", "study-kpi-track");
    track.appendChild(el("div", ""));

    card.append(header, value, footer, track);
    grid.appendChild(card);
});

root.appendChild(grid);
```

```dataviewjs
// ===== 3. QUICK ACTIONS =====
const CFG = JSON.parse(await dv.io.load("99_System/config.json"));

const moment = window.moment;
const now = moment();
const today = now.format("YYYY-MM-DD");
const week = now.format("GGGG-[W]WW");

const container = dv.container;
container.innerHTML = "";
const wrap = document.createElement("div");
wrap.className = "cc-action-wrap";
const grid = document.createElement("div");
grid.className = "cc-action-grid";
wrap.appendChild(grid);
container.appendChild(wrap);

async function ensureFolder(path) {
    let cur = "";
    for (const part of path.split("/").filter(Boolean)) {
        cur = cur ? `${cur}/${part}` : part;
        if (!app.vault.getAbstractFileByPath(cur)) {
            try { await app.vault.createFolder(cur); } catch (e) {}
        }
    }
}

async function openFile(file) {
    await app.workspace.getLeaf(false).openFile(file);
}

async function openOrCreate(path, content = "") {
    await ensureFolder(path.substring(0, path.lastIndexOf("/")));
    let file = app.vault.getAbstractFileByPath(path);
    if (!file) file = await app.vault.create(path, content);
    await openFile(file);
}

async function openBase(path) {
    const file = app.vault.getAbstractFileByPath(path);
    if (!file) return new Notice(`Base file not found: ${path}`);
    await openFile(file);
}

function createButton(icon, title, desc, action) {
    const b = document.createElement("button");
    b.className = "cc-action-button";
    b.innerHTML = `
        <span class="cc-action-icon">${icon}</span>
        <span class="cc-action-title">${title}</span>
        <span class="cc-action-desc">${desc}</span>
        <span class="cc-action-arrow">↗</span>`;
    b.addEventListener("click", async () => {
        b.classList.add("is-loading");
        try { await action(); }
        catch (e) { console.error(e); new Notice("Action failed: " + e.message); }
        finally { b.classList.remove("is-loading"); }
    });
    grid.appendChild(b);
}

// 1. PLAN TODAY
createButton("◷", "PLAN TODAY", "Create daily plan", () =>
    openOrCreate(`${CFG.folders.daily}/${today}.md`, `---
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
`));

// 2. CAPTURE IDEA
createButton("✳", "CAPTURE IDEA", "Save a new idea", () =>
    openOrCreate(`${CFG.folders.ideas}/${today}-${now.format("HH-mm-ss")}.md`, `---
type: idea
created: ${now.format("YYYY-MM-DD HH:mm")}
status: inbox
---

# New Idea

## Description

## Related Projects

## Notes
`));

// 3. START LEARNING
createButton("⌘", "START LEARNING", "Open active lesson", async () => {
    const re = new RegExp(CFG.patterns.lesson, "i");
    const lessons = dv.pages(`"${CFG.folders.courses}"`)
        .where(p => p.status === "learning" && re.test(p.file.name))
        .array()
        .sort((a, b) => Number(a.lesson_number ?? 0) - Number(b.lesson_number ?? 0));

    if (!lessons.length) return new Notice("No lesson has status: learning.");
    if (lessons.length > 1) new Notice("Multiple active lessons found. Opening the first one.");

    await openFile(app.vault.getAbstractFileByPath(lessons[0].file.path));
});

// 4. PROJECT REVIEW
createButton("▤", "PROJECT REVIEW", "Review active projects", () => openBase(CFG.bases.projects));

// 5. WEEKLY REVIEW
createButton("▥", "WEEKLY REVIEW", "Review the past 7 days", () =>
    openOrCreate(`${CFG.folders.weekly}/${week}.md`, `---
type: weekly-review
week: ${week}
start_date: ${moment().startOf("isoWeek").format("YYYY-MM-DD")}
end_date: ${moment().endOf("isoWeek").format("YYYY-MM-DD")}
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
`));

// 6. INBOX PROCESSING
createButton("▣", "INBOX PROCESSING", "Process unorganized notes", () => openBase(CFG.bases.inbox));

// 7. DEEP WORK (start / stop) — cùng schema với KPI: type, category, date, start, end
createButton("◉", "DEEP WORK", "Start / stop focus session", async () => {
    await ensureFolder(CFG.folders.sessions);
    const activePath = CFG.files.activeSession;
    const file = app.vault.getAbstractFileByPath(activePath);

    if (file) {
        const fm = app.metadataCache.getFileCache(file)?.frontmatter;
        if (fm?.status === "active") {
            const startM = moment(`${fm.date} ${fm.start}`, "YYYY-MM-DD HH:mm");
            const endM = moment();
            const minutes = Math.max(0, endM.diff(startM, "minutes"));

            await app.fileManager.processFrontMatter(file, f => {
                f.status = "completed";
                f.end = endM.format("HH:mm");
                f.duration_minutes = minutes;
            });

            const archive = `${CFG.folders.sessions}/${fm.date}_${String(fm.start).replace(":", "-")}_${fm.category ?? "deep_work"}.md`;
            await app.fileManager.renameFile(file, archive);

            return new Notice(`Deep Work completed: ${minutes} minutes.`);
        }
    }

    await app.vault.create(activePath, `---
type: ${CFG.types.session}
category: deep_work
date: ${today}
start: "${now.format("HH:mm")}"
end: ""
duration_minutes: 0
status: active
---

# Deep Work Session

## Objective

## Notes
`);
    new Notice("Deep Work session started.");
});

// 8. VAULT CLEANUP
createButton("⌁", "VAULT CLEANUP", "Maintain your workspace", () => openBase(CFG.bases.cleanup));
```

```dataviewjs
// ===== 4. SCHEDULE + DAILY TASKS =====
const CFG = JSON.parse(await dv.io.load("99_System/config.json"));

const root = dv.container;
root.innerHTML = "";

function el(tag, cls, text = "") {
    const n = document.createElement(tag);
    if (cls) n.className = cls;
    if (text !== "") n.textContent = text;
    return n;
}

// ---------- Schedule ----------
const scheduleFile = app.vault.getAbstractFileByPath(CFG.files.schedule);
let scheduleEvents = [];
let scheduleError = null;

if (scheduleFile) {
    try {
        const content = await app.vault.read(scheduleFile);
        scheduleEvents = content.split("\n")
            .map(line => {
                const m = line.match(/^\s*(?:[-*+]\s*)?(\d{2}:\d{2})\s*\|\s*(.+?)\s*$/);
                return m ? { time: m[1], title: m[2] } : null;
            })
            .filter(Boolean)
            .sort((a, b) => a.time.localeCompare(b.time));
    } catch (e) {
        console.error("Could not read schedule:", e);
        scheduleError = e;
    }
}

// ---------- Daily tasks ----------
const dailyPath = CFG.files.dailyTasks;
const dailyPage = dv.page(dailyPath.replace(/\.md$/, ""));
const dailyTasks = dailyPage
    ? (dailyPage.file.tasks ?? []).map(t => ({ ...t, path: dailyPage.file.path, completed: Boolean(t.completed) }))
    : [];

function updateProgress(tasks, count, percent) {
    const total = tasks.length;
    const done = tasks.filter(t => t.completed).length;
    const ratio = total ? Math.round(done / total * 100) : 0;
    count.textContent = `${done}/${total}`;
    percent.textContent = `${ratio}%`;
}

function renderTask(task, container, onUpdate) {
    const row = el("label", "workspace-task");
    const checkbox = el("input", "workspace-checkbox");
    checkbox.type = "checkbox";
    checkbox.checked = task.completed;

    const text = el("span", "workspace-task-text", task.text);
    if (task.completed) text.classList.add("is-completed");

    checkbox.addEventListener("change", async () => {
        const newState = checkbox.checked;
        const file = app.vault.getAbstractFileByPath(task.path);
        if (!file) { checkbox.checked = task.completed; return; }

        checkbox.disabled = true;
        try {
            await app.vault.process(file, content => {
                const lines = content.split("\n");
                const re = /^(\s*(?:[-*+]|\d+\.)\s*)\[([ xX])\](.*)$/;

                const candidates = [Number(task.line) - 1, Number(task.line)]
                    .filter((v, i, a) => Number.isInteger(v) && v >= 0 && v < lines.length && a.indexOf(v) === i);

                let idx = candidates.find(i => re.test(lines[i]) && lines[i].includes(task.text));
                if (idx === undefined) idx = candidates.find(i => re.test(lines[i]));
                if (idx === undefined) return content;

                lines[idx] = lines[idx].replace(re, (_, prefix, _old, rest) =>
                    `${prefix}[${newState ? "x" : " "}]${rest}`);
                return lines.join("\n");
            });

            task.completed = newState;
            text.classList.toggle("is-completed", newState);
            onUpdate();
        } catch (e) {
            console.error("Could not update task:", e);
            checkbox.checked = task.completed;
        } finally {
            checkbox.disabled = false;
        }
    });

    row.append(checkbox, text);
    container.appendChild(row);
}

function cardHeader(title, icon, countText) {
    const header = el("div", "workspace-card-header");
    const heading = el("div", "workspace-card-title");
    heading.append(el("span", "workspace-card-icon", icon), el("span", "", title));
    const count = el("span", "workspace-count", countText);
    header.append(heading, count);
    return { header, count };
}

function scheduleCard() {
    const card = el("section", "workspace-card schedule-card");
    const { header } = cardHeader("SCHEDULE", "◷", `${scheduleEvents.length} EVENTS`);
    const grid = el("div", "schedule-grid");

    if (!scheduleEvents.length) {
        const msg = !scheduleFile ? `${CFG.files.schedule} not found.`
                  : scheduleError ? "Could not read schedule file."
                  : "No schedule events found.";
        grid.appendChild(el("p", "workspace-empty", msg));
    } else {
        const left = el("div", "schedule-column");
        const right = el("div", "schedule-column");
        const mid = Math.ceil(scheduleEvents.length / 2);

        scheduleEvents.forEach((ev, i) => {
            const row = el("div", "schedule-event");
            row.append(el("span", "schedule-time", ev.time), el("span", "schedule-title", ev.title));
            (i < mid ? left : right).appendChild(row);
        });
        grid.append(left, right);
    }

    card.append(header, grid);
    return card;
}

function dailyCard() {
    const card = el("section", "workspace-card daily-card");
    const { header, count } = cardHeader("DAILY TASKS", "▣", "0/0");

    const stats = el("div", "workspace-stats");
    const percent = el("span", "workspace-percent", "0%");
    stats.append(percent, el("span", "workspace-caption", "DAILY COMPLETION"));

    const taskGrid = el("div", "workspace-task-grid");
    const refresh = () => updateProgress(dailyTasks, count, percent);

    dailyTasks.forEach(t => renderTask(t, taskGrid, refresh));
    refresh();

    card.append(header, stats, taskGrid);

    if (!dailyPage) card.appendChild(el("p", "workspace-empty", `${dailyPath} not found.`));
    else if (!dailyTasks.length) card.appendChild(el("p", "workspace-empty", "No tasks available."));

    return card;
}

const workspace = el("div", "workspace-grid");
workspace.append(scheduleCard(), dailyCard());
root.appendChild(workspace);
```

```dataviewjs
// ===== 5. COURSE STATUS =====
const CFG = JSON.parse(await dv.io.load("99_System/config.json"));
const lessonRe = new RegExp(CFG.patterns.lesson, "i");

const lessons = dv.pages(`"${CFG.folders.courses}"`)
    .where(p => lessonRe.test(p.file.name) && p.status === "learning")
    .array()
    .sort((a, b) => Number(a.lesson_number ?? 0) - Number(b.lesson_number ?? 0));

const root = dv.container;
root.innerHTML = "";

function create(tag, cls, text = "") {
    const n = document.createElement(tag);
    if (cls) n.className = cls;
    if (text !== "") n.textContent = text;
    return n;
}

const card = create("section", "course-status");
root.appendChild(card);

const header = create("div", "course-header");
const heading = create("div", "course-heading");
heading.append(create("span", "course-icon", "⌁"), create("span", "", "COURSE STATUS"));

const badge = create("span", "course-status-badge", lessons.length ? "LEARNING" : "NO ACTIVE LESSON");
if (!lessons.length) badge.classList.add("course-inactive");
header.append(heading, badge);
card.appendChild(header);

const current = create("div", "current-lesson");

if (lessons.length) {
    const page = lessons[0];
    const number = Number(page.lesson_number ?? 0);
    const total = Number(page.total_lessons ?? 0);
    const progress = total > 0 ? Math.min(100, number / total * 100) : 0;

    const top = create("div", "course-meta");
    top.append(
        create("span", "course-label", "CURRENT LESSON"),
        create("span", "course-number", total ? `${number} / ${total} LESSONS` : `LESSON ${number || "—"}`)
    );

    const info = create("div", "course-info");
    const left = create("div", "course-info-left");
    left.append(
        create("div", "course-lesson-name", page.file.name),
        create("div", "course-name", `${page.course ?? "Computer Vision"} · ${page.platform ?? "Learning"}`)
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
    bottom.append(create("span", "course-duration", `${page.duration ?? 0} MINUTES`));

    const button = create("button", "course-open", "OPEN CURRENT LESSON ↗");
    button.addEventListener("click", () => app.workspace.openLinkText(page.file.path, "", false));
    bottom.appendChild(button);

    current.append(top, info, track, bottom);
} else {
    current.append(
        create("div", "course-label", "CURRENT LESSON"),
        create("div", "course-lesson-name", "No active lesson"),
        create("div", "course-name", "Select a lesson and set status to learning.")
    );
}

card.appendChild(current);
```

```dataviewjs
// ===== 6. PROJECT DEADLINES =====
const CFG = JSON.parse(await dv.io.load("99_System/config.json"));

const todayDate = dv.date("today");

const projects = dv.pages(`"${CFG.folders.projects}"`)
    .where(p => p.type === CFG.types.project && p.status === "active" && p.deadline)
    .array()
    .map(p => {
        const deadline = dv.date(p.deadline);
        return {
            page: p,
            name: p.title ?? p.file.name,
            deadline,
            daysLeft: Math.ceil(deadline.startOf("day").diff(todayDate.startOf("day"), "days").days),
            progress: Number(p.progress ?? 0),
            priority: String(p.priority ?? "normal")
        };
    })
    .sort((a, b) => a.daysLeft - b.daysLeft);

function el(tag, cls, text = "") {
    const n = document.createElement(tag);
    if (cls) n.className = cls;
    if (text !== "") n.textContent = text;
    return n;
}

const root = dv.container;
root.innerHTML = "";

const card = el("section", "project-deadline-card");
const header = el("div", "project-deadline-header");
header.append(
    el("span", "project-deadline-title", "PROJECT DEADLINES"),
    el("span", "project-deadline-count", `${projects.length} ACTIVE`)
);
card.appendChild(header);

projects.forEach(pr => {
    const item = el("div", "deadline-project");
    const top = el("div", "deadline-project-top");

    let text, cls;
    if (pr.daysLeft < 0) { text = `${Math.abs(pr.daysLeft)} DAYS OVERDUE`; cls = "deadline-overdue"; }
    else if (pr.daysLeft === 0) { text = "DUE TODAY"; cls = "deadline-today"; }
    else { text = `${pr.daysLeft} DAYS LEFT`; cls = pr.daysLeft <= 7 ? "deadline-warning" : "deadline-normal"; }

    top.append(el("span", "deadline-project-name", pr.name), el("span", `deadline-badge ${cls}`, text));

    const meta = el("div", "deadline-project-meta");
    meta.append(
        el("span", "", pr.deadline.toFormat("dd MMM yyyy")),
        el("span", "", `${pr.priority.toUpperCase()} PRIORITY`),
        el("span", "", `${pr.progress}%`)
    );

    const track = el("div", "deadline-progress-track");
    const fill = el("div", "deadline-progress-fill");
    fill.style.width = `${Math.min(100, Math.max(0, pr.progress))}%`;
    track.appendChild(fill);

    const link = el("button", "deadline-open", "OPEN PROJECT ↗");
    link.addEventListener("click", () => app.workspace.openLinkText(pr.page.file.path, "", false));

    item.append(top, meta, track, link);
    card.appendChild(item);
});

if (!projects.length) card.appendChild(el("p", "workspace-empty", "No active projects found."));

root.appendChild(card);
```
