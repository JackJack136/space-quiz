# Space Exploration Quiz

A ten-question space exploration quiz with instant feedback, mission facts,
progress tracking, and a personal best saved in browser storage when available.
The app supports keyboard navigation, light and dark themes, and reduced motion.

## Play online

[Open Space Exploration Quiz](https://jackjack136.github.io/space-quiz/)

## Run locally

Clone or download this repository, then open `index.html` directly in a modern
browser (double-click the file or use the browser's **Open File** command).
No installation, build step, web server, or network connection is required.
All HTML, CSS, and JavaScript are contained in `index.html`, with no external
dependencies.

Choose an answer by clicking an option or pressing **1-4**. Use **Arrow Up** and
**Arrow Down** to move between options, and **Enter** or **Space** to advance after
answering. Select **Play Again** on the results screen to restart.

## Deployment

GitHub Pages uses **GitHub Actions** as its publishing source. The workflow in
`.github/workflows/pages.yml` checks that `index.html` is nonempty and packages
only that file as the site artifact. Pull requests targeting `main` run the
packaging job without publishing. Pushes to `main` publish the artifact to the
`github-pages` environment. The workflow can also be run manually from the
Actions tab on `main`.

No compilation is needed. Deployment uses the workflow's `GITHUB_TOKEN` and
OpenID Connect, so no repository secrets or personal access tokens are required.
