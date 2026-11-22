
```dataviewjs
// 1. Cấu hình
const folder = "02_Projects";

// 2. Tạo nút
const button = dv.el("button", "＋ New Project");

// 3. Xử lý khi nhấn nút
button.addEventListener("click", async () => {
    try {
        // Nhập tên Project
		const name = await new Promise((resolve) => {
		    const modal = new (class extends obsidian.Modal {
		        onOpen() {
		            const { contentEl } = this;
		            contentEl.createEl("h3", {
		                text: "New Project"
		            });
		
		            const input = contentEl.createEl("input", {
		                type: "text",
		                placeholder: "Project name"
		            });
		
		            input.style.width = "100%";
		
		            const button = contentEl.createEl("button", {
		                text: "Create"
		            });
		
		            button.style.marginTop = "12px";
		
		            button.onclick = () => {
		                resolve(input.value);
		                this.close();
		            };
		
		            input.focus();
		
		            input.addEventListener("keydown", (e) => {
		                if (e.key === "Enter") button.click();
		            });
		        }
		
		        onClose() {
		            this.contentEl.empty();
		            resolve(null);
		        }
		    })(app);
		
		    modal.open();
		});

if (!name || !name.trim()) return;

        // Làm sạch tên file
        const safeName = name.trim()
            .replace(/[\\/:*?"<>|]/g, "-")
            .replace(/[. ]+$/g, "");

        if (!safeName) return;

        const filePath = `${folder}/${safeName}.md`;

        // Kiểm tra file đã tồn tại
        if (app.vault.getAbstractFileByPath(filePath)) {
            new Notice("Project đã tồn tại!");
            return;
        }

        // Tạo thư mục nếu chưa có
        if (!app.vault.getAbstractFileByPath(folder)) {
            await app.vault.createFolder(folder);
        }

        // Lấy ngày hiện tại theo giờ máy
        const now = new Date();
        const date = [
            now.getFullYear(),
            String(now.getMonth() + 1).padStart(2, "0"),
            String(now.getDate()).padStart(2, "0")
        ].join("-");

        // Nội dung file
        const content = `---
type: project
status: planning
priority: medium
created: ${date}
tags:
  - project
---

# ${safeName}

## Objective

## Tasks

## Notes
`;

        // Tạo file
        const file = await app.vault.create(filePath, content);

        // Mở file vừa tạo
        await app.workspace.getLeaf(true).openFile(file);

        new Notice("Đã tạo Project!");

    } catch (error) {
        console.error(error);
        new Notice("Không thể tạo Project: " + error.message);
    }
});
```
