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
