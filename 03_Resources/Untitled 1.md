```dataviewjs
// This goes inside a ```dataviewjs code block in your note

// Create a button element
const btn = document.createElement("button");
btn.textContent = "Click Me"; // Button label
btn.style.padding = "6px 12px";
btn.style.backgroundColor = "#4CAF50";
btn.style.color = "white";
btn.style.border = "none";
btn.style.borderRadius = "4px";
btn.style.cursor = "pointer";

// Add click event
btn.addEventListener("click", () => {
    // Example action: show a popup
    new Notice("Button clicked!");
    
    // You can also run custom JS, open files, etc.
    // app.workspace.openLinkText("MyNote.md", "", false);
});

// Append button to Dataview container
dv.container.appendChild(btn);

```

