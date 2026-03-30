# TabPad

A desktop app for extracting guitar tab sheets from YouTube videos and exporting them as print-ready PDFs.

Paste a YouTube URL, trim to the tab section, crop the frame, and TabPad automatically captures each unique tab position. Export with custom layout options including panorama stitch and adjustable line spacing.

---

## Getting Started

### Download

Download the latest `TabPad.exe` from the [Releases](../../releases) page. No installation required — just run the file.

### Running from Source

```bash
pip install customtkinter pillow opencv-python yt-dlp
python main.py
```

---

## Walkthrough

TabPad is split into five steps, accessible from the sidebar.

---

### 1 · Download

Paste a YouTube URL and download the video, or load a local file you already have.

| Option | Description |
|---|---|
| **YouTube URL** | Paste the full video URL |
| **Save to** | Folder where the video is saved. Defaults to `Downloads\TabPad` |
| **Download** | Fetches the video at 360p using the Android player client (most reliable, no login required) |
| **Use Local File** | Skip downloading — point TabPad at a video already on your computer |

> **Higher quality** is not available without YouTube authentication. For most tab videos, 360p is sufficient to read the notation clearly.

---

### 2 · Trim

Scrub through the video and set a start and/or end point to cut out intros, outros, or any section that doesn't contain tab.

| Control | Description |
|---|---|
| **Scrubber** | Drag to navigate through the video frame by frame |
| **Set start** | Marks the current frame as the beginning of the tab section |
| **Set end** | Marks the current frame as the end of the tab section |
| **Clear** | Removes both markers and resets to the full video |

The orange bar on the timeline shows the active trim region. Processing will only run within this range.

---

### 3 · Crop

Draw a box over the tab area to tell TabPad exactly where the tab notation lives on screen. This excludes any UI chrome, video titles, or background from the captured frames.

| Control | Description |
|---|---|
| **Drag** | Click and drag on the video to draw a crop region |
| **Scrubber** | Navigate to a frame where the tab is clearly visible before drawing |
| **Confirm Crop** | Locks in the drawn region |
| **Clear** | Removes the current crop selection |

---

### 4 · Process

TabPad scans the trimmed, cropped video and automatically extracts only the unique frames — skipping duplicates and mid-transition frames.

| Setting | Description |
|---|---|
| **Change threshold** | How different a frame must be from the last saved one to be captured. Lower = more sensitive, captures more frames. Higher = only captures significant changes |
| **Sample every N frames** | How frequently the video is sampled. Higher = faster processing but may miss frames |
| **Colour filter** | Apply **None**, **Grayscale**, or **Black & White** to all captured frames |
| **B&W threshold** | When Black & White is selected, controls how aggressively pixels are whitened. Higher = more detail preserved |

#### Managing Frames

Once processing is complete, all captured frames are shown as a scrollable list.

- **Click a frame** to toggle it included or excluded from the export
- **Drag the orange handles** on either side of a frame to crop it horizontally (useful if the video has side panels or UI elements that weren't caught by the global crop)
- **Reset crop** restores a frame's horizontal crop to full width
- Excluded frames are dimmed and shown with a strikethrough label

---

### 5 · Export

Configure the PDF layout and generate the final document.

| Setting | Description |
|---|---|
| **Gap between tabs** | Vertical space between each tab row on the page (mm) |
| **Page margin** | Space around the edges of each page (mm) |
| **Page size** | A4, Letter, or A3 |
| **Tab name** | Title printed at the top of the first page |
| **arr. by** | Arranger or channel name printed below the title. Uncheck to hide |
| **Filename** | Name of the output `.pdf` file |
| **Lock line height** | Forces all rows to the same height, letterboxing any frames that are a different aspect ratio |
| **Panorama stitch** | Joins all frames into one continuous horizontal strip, then cuts it into rows at the page width — keeps bars together and avoids awkward mid-note line breaks |

PDFs are saved to `Downloads\TabPad\` by default.

#### Advanced Settings

Click **Advanced settings** to expose fine-grained layout controls:

| Setting | Default | Description |
|---|---|---|
| Top padding | 8 mm | Space above the title on page 1 |
| Title gap | 3 mm | Space between the title and the divider line |
| Divider gap | 4 mm | Space between the divider line and the first tab row |
| Footer height | 14 mm | Height of the footer area at the bottom of each page |
| Logo height | 20 mm | Height of the logo in the footer |
| Logo margin | 10 mm | Right margin of the footer logo |
| Title font size | 60 pt | |
| Channel font size | 26 pt | |
| Page no. font size | 20 pt | |
| Output DPI | 150 | Resolution of the rendered PDF |
| Line height (mm) | — | Force a fixed row height. Activates panorama stitch automatically |
| Lines per page | — | Force a fixed number of rows per page. Tab height is calculated to fit. Activates panorama stitch automatically |

---

## Settings

All settings are automatically saved when you close the app and restored on next launch. This includes processing thresholds, export options, output directory, and advanced layout values.

---

## Appearance

Use the **Appearance** switcher at the bottom of the sidebar to toggle between **Dark**, **Light**, and **System** modes.

---

## Building from Source

To produce a standalone `TabPad.exe`:

```bash
pip install pyinstaller
pyinstaller TabPad.spec --clean --noconfirm
```

Or double-click `build.bat`. The finished executable will be in the `dist\` folder.
