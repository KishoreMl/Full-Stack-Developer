1. Virtualize the projects list
    - In virtualization only visible items are rendered to the DOM when the dataset is much larger.
    - Instead of rendering all projects at once (e.g., hundreds), only the projects currently visible in the viewport (and maybe a small buffer) are rendered. As the user scrolls, items enter and exit the DOM dynamically.

2. Lazy Load the Project Thumbnail images
    - Lazy loading delays the loading of images until they are about to enter the viewport.
    - Images that are off-screen are not fetched or rendered until needed.

These are things that can be done as part of UI changes for projects list page.
