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
const pages = dv.pages('"01 Journal/Daily"');

const today = dv.date("today");
const weekStart = today.startOf("week");
const weekEnd = weekStart.plus({ days: 7 });

const daily = pages.where(p => {
    const rawDate = p.date ?? p.file.day;
    if (!rawDate) return false;

    const date = dv.date(rawDate);

    return date &&
        date >= weekStart &&
        date < weekEnd;
});

const sum = (field) =>
    daily.array().reduce(
        (total, p) => total + Number(p[field] ?? 0),
        0
    );

const metrics = [
    {
        label: "DEEP WORK",
        value: (sum("deep_work_minutes") / 60).toFixed(1),
        unit: "h",
        target: 15,
        color: "orange",
        suffix: "HOURS THIS WEEK"
    },
    {
        label: "FOCUS SESSIONS",
        value: sum("focus_sessions"),
        unit: "",
        target: 20,
        color: "green",
        suffix: "SESSIONS COMPLETED"
    },
    {
        label: "EXPERIMENTS",
        value: sum("experiments_completed"),
        unit: "",
        target: 4,
        color: "purple",
        suffix: "PRACTICAL EXPERIMENTS"
    },
    {
        label: "RESEARCH REVIEWS",
        value: sum("research_reviews"),
        unit: "",
        target: 5,
        color: "yellow",
        suffix: "PAPERS / DOCUMENTS"
    }
];

const root = dv.el("div", "", {
    cls: "study-kpi-grid"
});

root.innerHTML = metrics.map(m => {
    const value = Number(m.value);
    const percent = Math.min(
        100,
        Math.round(value / m.target * 100)
    );

    return `
        <div class="study-kpi-card ${m.color}">
            <div class="study-kpi-header">
                <span>${m.label}</span>
                <span class="study-kpi-status"></span>
            </div>

            <div class="study-kpi-value">
                ${m.value}<small>${m.unit}</small>
            </div>

            <div class="study-kpi-footer">
                <span>${percent}%</span>
                <span>${m.suffix}</span>
            </div>

            <div class="study-kpi-track">
                <div style="width:${percent}%"></div>
            </div>
        </div>
    `;
}).join("");

```
```dataviewjs
const page = dv.page("02_Areas/Lesson.md");

const root = dv.container;

function create(tag, cls, text = "") {
    const node = document.createElement(tag);
    if (cls) node.className = cls;
    if (text !== "") node.textContent = text;
    return node;
}

root.empty();

if (!page) {
    root.createEl("p", {
        text: "Current Lesson.md not found."
    });
} else {
    const course = page.course ?? "Untitled Course";
    const lesson = page.lesson ?? "No current lesson";
    const number = Number(page.lesson_number ?? 0);
    const total = Number(page.total_lessons ?? 0);
    const duration = page.duration ?? 0;
    const progress = total > 0
        ? Math.min(100, Math.max(0, number / total * 100))
        : 0;

    const status = page.status ?? "learning";
    const playlistId = page.playlist_id ?? "";
    const lessonUrl = page.lesson_url ?? "";
    const courseUrl = page.course_url ?? "";

    // MAIN CARD
    const card = create("section", "course-status");
    root.appendChild(card);

    // HEADER
    const header = create("div", "course-header");

    const heading = create("div", "course-heading");
    const icon = create("span", "course-icon", "⌁");
    const title = create("span", "", "COURSE STATUS");

    heading.append(icon, title);

    const badge = create(
        "span",
        "course-status-badge",
        status.toUpperCase()
    );

    header.append(heading, badge);
    card.appendChild(header);

    // CURRENT LESSON
    const current = create("div", "current-lesson");

    const top = create("div", "course-meta");
    top.append(
        create("span", "course-label", "CURRENT LESSON"),
        create(
            "span",
            "course-number",
            `${number} / ${total} LESSONS`
        )
    );

    const info = create("div", "course-info");

    const left = create("div", "course-info-left");
    left.append(
        create("div", "course-lesson-name", lesson),
        create("div", "course-name", `${course} · ${page.platform ?? ""}`)
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

    const durationText = create(
        "span",
        "course-duration",
        `${duration} MINUTES`
    );

    const lessonLink = create("a", "course-open", "OPEN CURRENT LESSON ↗");
    lessonLink.href = lessonUrl || courseUrl || "#";
    lessonLink.target = "_blank";
    lessonLink.rel = "noopener noreferrer";

    bottom.append(durationText, lessonLink);

    current.append(top, info, track, bottom);
    card.appendChild(current);
}
```
```dataviewjs
// ======================================
// DAILY TASKS + PERSONAL PROJECTS
// ======================================

const dailyPage = dv.page("05 Tasks/Daily Tasks.md");

const projectPages = dv.pages('"03 Projects"')
    .where(p =>
        p.type === "project" &&
        p.status === "active"
    )
    .sort(p => p.file.name, "asc")
    .array();

// ---------- DOM HELPERS ----------

function el(tag, className, text = "") {
    const node = document.createElement(tag);
    if (className) node.className = className;
    if (text !== "") node.textContent = text;
    return node;
}

function getTasks(page) {
    if (!page) return [];
    return (page.file.tasks ?? []).map(task => ({
        ...task,
        path: page.file.path,
        completed: Boolean(task.completed)
    }));
}

// ---------- TASK DATA ----------

const dailyTasks = getTasks(dailyPage);

const projects = projectPages.map(page => ({
    name: page.title ?? page.file.name,
    path: page.file.path,
    tasks: getTasks(page)
}));

// ---------- MAIN LAYOUT ----------

const workspace = el("div", "workspace-grid");
dv.container.appendChild(workspace);

// ---------- PROGRESS ----------

function updateProgress(tasks, progress, count, percent) {
    const total = tasks.length;
    const completed = tasks.filter(t => t.completed).length;
    const ratio = total ? Math.round(completed / total * 100) : 0;

    progress.style.width = `${ratio}%`;
    count.textContent = `${completed}/${total}`;
    percent.textContent = `${ratio}%`;
}

// ---------- TASK RENDERER ----------

function renderTask(task, container, onUpdate) {
    const row = el("label", "workspace-task");

    const checkbox = el("input", "workspace-checkbox");
    checkbox.type = "checkbox";
    checkbox.checked = task.completed;

    const text = el("span", "workspace-task-text", task.text);

    if (task.completed) {
        text.classList.add("is-completed");
    }

    checkbox.addEventListener("change", async () => {
        const newState = checkbox.checked;
        const file = app.vault.getAbstractFileByPath(task.path);

        if (!file) {
            checkbox.checked = task.completed;
            return;
        }

        checkbox.disabled = true;

        try {
            await app.vault.process(file, content => {
                const lines = content.split("\n");
                const line = lines[task.line];

                if (line === undefined) return content;

                const updated = line.replace(
                    /^(\s*[-*]\s*)\[([ xX])\]/,
                    (_, prefix) => `${prefix}[${newState ? "x" : " "}]`
                );

                lines[task.line] = updated;
                return lines.join("\n");
            });

            task.completed = newState;
            text.classList.toggle("is-completed", newState);
            onUpdate();

        } catch (error) {
            console.error("Could not update task:", error);
            checkbox.checked = task.completed;
        } finally {
            checkbox.disabled = false;
        }
    });

    row.append(checkbox, text);
    container.appendChild(row);
}

// ---------- CARD HEADER ----------

function createCardHeader(title, iconName, colorClass) {
    const header = el("div", "workspace-card-header");

    const heading = el("div", "workspace-card-title");
    const icon = el("span", "workspace-card-icon", iconName);
    const label = el("span", "", title);

    heading.append(icon, label);

    const count = el("span", "workspace-count", "0/0");
    header.append(heading, count);

    return { header, count };
}

// ---------- DAILY TASK CARD ----------

function createDailyCard() {
    const card = el("section", "workspace-card daily-card");

    const { header, count } =
        createCardHeader("DAILY TASKS", "▣");

    const stats = el("div", "workspace-stats");
    const percent = el("span", "workspace-percent", "0%");
    const caption = el("span", "workspace-caption", "DAILY COMPLETION");

    stats.append(percent, caption);

    const track = el("div", "workspace-progress-track");
    const fill = el("div", "workspace-progress-fill daily-fill");
    track.appendChild(fill);

    const taskGrid = el("div", "workspace-task-grid");

    dailyTasks.forEach(task => {
        renderTask(task, taskGrid, () =>
            updateProgress(dailyTasks, fill, count, percent)
        );
    });

    card.append(header, stats, track, taskGrid);

    updateProgress(dailyTasks, fill, count, percent);

    if (!dailyPage) {
        card.appendChild(
            el("p", "workspace-empty", "Daily Tasks.md not found.")
        );
    }

    return card;
}

// ---------- PROJECT CARD ----------

function createProjectCard() {
    const card = el("section", "workspace-card project-card");

    const { header, count } =
        createCardHeader("PERSONAL PROJECTS", "▤");

    const allTasks = projects.flatMap(p => p.tasks);

    const stats = el("div", "workspace-stats");
    const percent = el("span", "workspace-percent", "0%");
    const caption = el("span", "workspace-caption", "PROJECT COMPLETION");

    stats.append(percent, caption);

    const track = el("div", "workspace-progress-track");
    const fill = el("div", "workspace-progress-fill project-fill");
    track.appendChild(fill);

    card.append(header, stats, track);

    if (projects.length === 0) {
        card.appendChild(
            el("p", "workspace-empty", "No active projects found.")
        );
    }

    projects.forEach(project => {
        const section = el("div", "workspace-project");

        const projectHeader = el("div", "workspace-project-header");
        const name = el("span", "workspace-project-name", project.name);
        const taskCount = el(
            "span",
            "workspace-project-count",
            `${project.tasks.filter(t => t.completed).length}/${project.tasks.length}`
        );

        projectHeader.append(name, taskCount);

        const taskGrid = el("div", "workspace-task-grid");

        project.tasks.forEach(task => {
            renderTask(task, taskGrid, () => {
                taskCount.textContent =
                    `${project.tasks.filter(t => t.completed).length}/${project.tasks.length}`;

                updateProgress(allTasks, fill, count, percent);
            });
        });

        section.append(projectHeader, taskGrid);
        card.appendChild(section);
    });

    updateProgress(allTasks, fill, count, percent);

    return card;
}

// ---------- RENDER ----------

workspace.append(
    createDailyCard(),
    createProjectCard()
);
```
