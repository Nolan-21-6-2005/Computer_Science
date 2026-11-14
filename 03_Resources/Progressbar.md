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
