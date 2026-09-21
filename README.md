<div align="center">

<img src="assets/icons/android-chrome-192x192.png" alt="The Cube icon" width="88" />

THE CUBE

A classic puzzle. A fresh perspective.

An interactive 3D Rubik’s Cube game with custom themes, smooth turns, and personal solve records.






Explore features · Run locally · How to play · Contribute · Support

</div>

ROTATE · SCRAMBLE · SOLVE
Four cube sizes. Five colour presets. Your next personal best.



[!NOTE]
This repository builds on the original work of Boris Sehovac. Repository customisation and maintenance: SULAKSHAN M. Original credits are preserved below.

Overview

The Cube brings the classic colour-matching puzzle to an interactive 3D environment. Turn cube layers, explore different cube sizes, customise the appearance, and track your solving times directly in your browser.
The application uses Three.js for rendering and vanilla JavaScript for gameplay. The supplied project includes compiled JavaScript and CSS, so a local static web server is enough to serve the game without building the source first.

Features

Feature

Description

Interactive 3D cube

Drag-based controls for turning layers and rotating the cube

Four cube sizes

Choose 2×2×2, 3×3×3, 4×4×4, or 5×5×5

Scrambling

Three scramble-length settings, with lengths varying by cube size

Flip animations

Swift, Smooth, and Bounce styles

Adjustable camera

Change the view between an orthographic-like appearance and a stronger perspective

Colour schemes

Cube, Erno, Dust, Camo, and Rain presets

Theme editor

Adjust hue, saturation, and lightness

Solve timer

Track the duration of each solve

Statistics

Total solves, best time, worst time, and averages over 5, 12, and 25 solves

Local persistence

Store preferences, scores, and an in-progress game using browser localStorage

Completion effects

Animated celebration and best-time feedback

Mouse and touch input

Controls implemented for desktop and touch interaction

Statistics are recorded separately for each cube size. The displayed averages are arithmetic means of the most recent solves, rather than competition-style trimmed averages.

Technology

Layer

Technology

Page structure

HTML5

Styling

Compiled CSS with Sass source

Game logic

Vanilla JavaScript, organised into modules

3D rendering

Bundled Three.js and WebGL

Persistence

Browser localStorage

JavaScript bundling

Rollup configuration files

Build minification

rollup-plugin-babel-minify

No backend, database, API key, or account is required for the core game.

Run locally

Requirements

A browser with WebGL enabled.

Python 3, or another static HTTP server.

Start the included application

Download and extract the project.

Open a terminal in the folder containing index.html.

Start a local web server:

Windows:

cd the-cube-master
py -m http.server 8000

macOS / Linux:

cd the-cube-master
python3 -m http.server 8000

If your terminal is already inside the project folder, skip the cd command.

Open http://localhost:8000.

Double-click or double-tap the start area to begin.

Press Ctrl+C in the terminal to stop the server.

The supplied archive references missing UpUp offline-support files. See Known limitations below for the resulting console error and how to address it. The game scripts are loaded before that offline-support block.

How to play

Open settings to choose a cube size, scramble length, animation style, and theme.

Return to the start screen and double-click or double-tap to start.

Drag across a cube face to turn a layer.

Drag outside the cube to rotate the view of the cube.

Restore every face to a single colour.

Open the trophy/statistics view to review your results.

Use the theme editor to personalise the colours. Preferences and progress are stored in the current browser; they do not synchronise across devices.

Project structure

<details>
<summary><strong>Explore the source files</strong></summary>

Path

Purpose

index.html

Main page, interface markup, metadata, and script loading

assets/css/styles.css

Compiled application styles

assets/js/three.js

Bundled Three.js library

assets/js/cube.js

Compiled game code loaded by the page

assets/icons/

Favicons, application icons, preview images, and manifest

src/js/Game.js

Main application entry point and game coordination

src/js/Cube.js

Cube creation and state

src/js/Controls.js

Cube interaction and rotation logic

src/js/World.js

3D scene, camera, and renderer

src/js/Scrambler.js

Scramble generation

src/js/Timer.js

Solve timing

src/js/Scores.js

Solve records and statistics

src/js/Storage.js

Browser persistence

src/js/Preferences.js

Settings controls

src/js/Themes.js

Colour presets

src/js/ThemeEditor.js

Colour customisation

src/js/Confetti.js

Completion celebration

src/js/plugins/

Custom rounded geometry helpers

src/scss/

Sass styling source

rollup.config.dev.js

Development JavaScript bundle configuration

rollup.config.build.js

Minified JavaScript bundle configuration

package.json

Legacy package metadata and scripts

</details>

Development notes

The browser loads assets/js/cube.js, not the individual modules inside src/js/. Source changes therefore need to be bundled before they appear in the game.
Likewise, changes in src/scss/ must be compiled to assets/css/styles.css.
The supplied development Rollup configuration uses src/js/Game.js as its entry point and outputs an immediately invoked bundle to assets/js/cube.js. The production configuration adds minification.

Existing npm scripts

Command

Status in the supplied project

npm start

No start script is defined

npm run dev

No dev script is defined

npm run build

Legacy script; requires repairs before use

npm test

Placeholder that deliberately exits with an error

Running the existing compiled application does not require npm install.

Static hosting

The runtime application consists of index.html and the assets/ directory. Serve these together from the same root, preserving their relative paths.
Before publishing:

Address the missing offline-support scripts described below.

Update the original site URL and social-preview metadata in index.html for your deployment.

Remove or replace the original Google Analytics snippet with your own configuration, if analytics are wanted.

Preserve the original author credit and clarify any modifications you make.

No server-side runtime or environment variables are needed for the core game. Do not rely on the existing npm build command as a deployment step until it has been repaired.

Known limitations

<details>
<summary><strong>Read the legacy build and runtime notes</strong></summary>

Missing offline-support files

index.html requests upup.min.js, but that file is absent from the supplied archive. The build script also expects upup.sw.min.js, which is absent.
The check if (UpUp !== null) does not safely handle an undeclared variable and can produce ReferenceError: UpUp is not defined.
For an online-only version, remove the upup.min.js script tag and its associated UpUp.start(...) block. To retain offline support, restore the compatible UpUp assets and verify the caching setup. Offline operation is not complete in this archive.

Incomplete legacy build setup

The build script invokes rollup, but Rollup itself is not declared in the supplied development dependencies. It then copies the missing UpUp files into export/ and assumes the output directory exists. Its Unix-style cp commands are also not portable to a default Windows command shell.
Repair the dependencies, output-directory creation, and asset-copy steps before using this script. The committed assets/ files provide a separate way to run the existing game without rebuilding.

Keyboard controls

Keyboard-related source files exist, but the keyboard import in Game.js is commented out. Keyboard gameplay should not be treated as an enabled feature of this version.

Local storage

Clearing browser site data removes saved preferences, scores, and progress. Changing browser, device, hostname, or port uses a different storage context. A game-version change can also reset saved game state and preferences.

Score history typo

Scores.js uses data.scores.lenght instead of data.scores.length in the intended score-history limit check. The intended cap of 100 saved times therefore is not enforced by that code.

</details>

Troubleshooting

Problem

What to check

Blank page or missing cube

Inspect browser console errors, WebGL availability, and requests for the bundled scripts

Missing styles or icons

Serve the folder containing index.html and preserve the assets/ paths

UpUp is not defined

Remove the unused offline block or restore its missing dependencies

Missing script: start

Use a static server; this project has no npm start script

Build fails

Review the incomplete build setup above

Source edits do not appear

Rebuild the relevant JavaScript or stylesheet, then refresh

Saved records seem missing

Use the same browser and site address, and check whether site data was cleared

Contributing

Bug reports, documentation improvements, and focused pull requests are welcome.

Check existing issues and pull requests for related work.

For a bug, include your browser, device, cube size, reproduction steps, and any console error.

For a change, create a branch and keep the pull request focused on one improvement.

Explain what changed and how you checked it. Include before-and-after images for visual updates.

Good starting points include the score-history typo, the missing offline assets, and the legacy build setup described above.

Ideas for future improvements

These are proposed improvements, not current features or delivery promises.

Repair and document the JavaScript and Sass build workflow.

Restore or remove incomplete offline support.

Add and verify keyboard controls.

Improve accessibility and reduced-motion support.

Add move counting and optional sound controls.

Export and import local solve statistics.

Support the project

If you enjoy this project, give the repository a star, share it with a puzzle enthusiast, or contribute a useful improvement. Clear bug reports and thoughtful feedback help too.

Buy me a coffee



Want to support SULAKSHAN M’s work on this repository? A personal support link will be added here once it is configured.

<!-- Replace the unlinked badge and the sentence above with a linked badge when
     your actual Buy Me a Coffee profile is available:
     [![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-FFDD00?style=for-the-badge&logo=buymeacoffee&logoColor=000000)](YOUR_VERIFIED_BUY_ME_A_COFFEE_URL)
     Do not publish the placeholder as a real destination. -->

Validation scope

This README is based on inspection of the supplied project source and package configuration. It does not claim that browser compatibility, gameplay, deployment, or a repaired build pipeline has been tested.

Credits and licence

The supplied HTML identifies Boris Sehovac as the original author. Its metadata references the original bsehovac GitHub project.
package.json declares ISC, but the supplied archive does not contain a standalone licence file. Preserve existing attribution and confirm the applicable upstream licence text before redistributing the project. Bundled third-party components may have their own licence notices.

<div align="center">

Enjoy the challenge? Star the repository and share your best solve.

Customised by SULAKSHAN M · Original game by Boris Sehovac

Back to top

</div>
