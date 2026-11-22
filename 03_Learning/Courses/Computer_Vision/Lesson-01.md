---
status: learning
course: Computer Vision
lesson_number: 1
duration: 25
lesson_url: https://youtube.com/watch?v=VIDEO_ID
---

```dataviewjs
const playlistId = "PLKikgSZ8GZYd7EXmKZSLwITvoCm0KFI5B";

const container = dv.container;

const wrapper = document.createElement("div");
wrapper.className = "youtube-playlist";

const iframe = document.createElement("iframe");

iframe.src =
    `https://www.youtube-nocookie.com/embed/videoseries?list=${playlistId}`;

iframe.title = "Computer Vision Playlist";
iframe.allow =
    "accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share";

iframe.allowFullscreen = true;
iframe.loading = "lazy";

wrapper.appendChild(iframe);
container.appendChild(wrapper);
```

