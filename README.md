# UngapSRT

Tired of awkward silences in your subtitles? This tool zaps those tiny, annoying gaps between subtitle lines, making your viewing experience smooth like butter. All wrapped up in a funky, retro ASCII-themed interface as always :)

<img width="1603" height="865" alt="image" src="https://github.com/user-attachments/assets/de4e06a0-139f-437d-a682-166033ce1455" />


## Features

* **Automatic Gap Removal**: Intelligently merges subtitle timings to remove gaps smaller than a specified threshold.

* **Adjustable Threshold**: Use a slider or number input to control the maximum gap (from 0.1s to 5s) that gets merged. The results update in real time!

* **Batch Processing**: Drag and drop multiple `.srt` files at once.

* **Purely Client-Side**: No server needed! All the processing happens securely in your browser using vanilla JavaScript.

* **Groovy ASCII Theme**: A fun, retro terminal aesthetic with both light and dark modes.

* **Easy Download**: Instantly download the processed files, conveniently renamed with a `_merged` suffix.

## How to Use

It's as simple as it gets. No build process required.

1. Clone the repository.

2. Open the `index.html` file in your favorite web browser.

3. Drag & drop your `.srt` file(s) onto the drop zone.

4. Adjust the "MAX GAP TO MERGE" slider to your preference.

5. Click the "DOWNLOAD" button for each processed file. Voilà!

## Code Deep Dive

### High-Level Project Overview

`SRT_GAP_REMOVER` is a lightweight, single-page web application built with vanilla **HTML, CSS, and JavaScript**. There are no frameworks, no dependencies, and no backend. This makes it incredibly fast, portable, and secure, as all file processing is handled directly in the user's browser.

The application is contained entirely within a single `index.html` file, which is divided into three logical parts:

* **HTML**: Defines the structure and content of the user interface.

* **CSS (`<style>` block)**: Handles all the styling, including the cool ASCII theme, animations, and the dynamic light/dark modes using CSS variables.

* **JavaScript (`<script>` block)**: Contains all the logic for file handling, SRT parsing, and DOM manipulation.

<details>
<summary>[The Magic Inside]</summary>

The core logic of the application resides in the JavaScript. Here's how it works:

* **Time Conversion (`timeToMs` & `msToTime`)**:
  Two utility functions are the backbone of the time manipulation.

  * `timeToMs`: Takes an SRT timestamp string (`HH:MM:SS,ms`) and converts it into a total millisecond count. This makes calculating time differences straightforward.

  * `msToTime`: Does the reverse, converting a millisecond value back into a perfectly formatted SRT string.

* **The Core Logic (`processSrt`)**:
  This is where the main subtitle processing happens. When a file is read, its content is passed to this function along with the user-defined threshold.

  1. The entire SRT file content is split into individual subtitle blocks based on the double newline separator (`\r?\n\r?\n`).

  2. Each block is then mapped into a JavaScript object containing its `index`, `startTime`, `endTime`, and `text`. The start and end times are also converted to milliseconds for easy comparison.

  3. The code then loops through the array of subtitle objects, starting from the second one (`i = 1`).

  4. In each iteration, it calculates the `gap` between the `startTimeMs` of the current subtitle and the `endTimeMs` of the previous one.

  5. **If the `gap` is greater than 0 but less than or equal to the `thresholdMs`**, it means the gap should be removed! The function adjusts the current subtitle's `startTimeMs` to be just 1ms after the previous subtitle's `endTimeMs`.

  6. Finally, the array of modified subtitle objects is joined back together into a single, correctly formatted SRT string.

* **DOM Manipulation & Event Handling**:
  The UI is brought to life with event listeners.

  * A `drop` event listener on the `drop-zone` element handles the drag-and-drop functionality.

  * `input` event listeners on the `threshold-slider` and `threshold-number` immediately re-run the `processFiles` function whenever the user changes the gap value.

  * The `processFiles` function reads the selected files using `FileReader` and then calls `createResultCard` to dynamically generate and animate the result cards in the UI.

* **Theming (`applyTheme`)**:
  The light/dark mode is controlled by toggling a `dark` class on the root `<html>` element. The CSS uses variables (e.g., `--bg-color`, `--text-color`) that are redefined within the `html.dark` scope, allowing for an instant theme switch. The user's preference is saved to `localStorage` so it persists across sessions.
</details>

## Contributing

Got an idea to make this even better? Feel free to fork the repo, make your changes, and submit a pull request. Collaborations, improvements, and suggestions are always welcome!

## Credits

* **Built by Navaneeth Sankar K P** ([GitHub Profile](https://github.com/navuxneeth) | [LinkedIn](https://www.linkedin.com/in/navaneeth-sankar-k-p))

* **AI Assistance**: The code was generated with the help of Google DeepMind's Gemini 2.5 Pro.

