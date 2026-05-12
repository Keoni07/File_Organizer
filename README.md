# File_Organizer

# File Organizer

This file organizer automatically sorts downloaded files into categorized folders based on their file extension.

If a folder for a specific file extension is listed in `list_of_directories`, the folder is generated automatically and the file is moved into it. If there is no matching folder for a file's extension, the file is skipped and left in place — keeping the script running without interruption.

---

## How It Works

1. The script scans the specified source directory (default: `~/Downloads`)
2. Each file's extension is read and matched against a dictionary of known categories
3. If a match is found, the destination folder is created (if it doesn't already exist) and the file is moved into it
4. If no match is found, the file is ignored and the script moves on

---

## Supported File Types

| Folder | Extensions |
|---|---|
| Images | `.jpg` `.jpeg` `.png` `.gif` `.bmp` `.tiff` |
| Videos | `.mp4` `.avi` `.mkv` `.mov` |
| Zip | `.zip` `.rar` `.7z` |
| Audio | `.mp3` `.wav` `.flac` |
| Documents | `.docx` `.xlsx` `.txt` `.csv` `.pptx` |
| PDF | `.pdf` |

---

## Requirements

- Python 3.14
- macOS (required for LaunchAgent automation)
- Libraries: `os`, `pathlib`, `shutil` — all built into Python, nothing to install

---

## Usage

### Run manually

```bash
python3 /path/to/File_Organizer.py
```

### Run automatically with a LaunchAgent (macOS)

To have the script run daily at 9am without any manual input, set it up as a macOS LaunchAgent:

1. Create the log directory:
```bash
mkdir ~/logs
```

2. Save the following as `~/Library/LaunchAgents/com.yourname.fileorganizer.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>

    <key>Label</key>
    <string>com.yourname.fileorganizer</string>

    <key>StandardOutPath</key>
    <string>/Users/yourusername/logs/fileorganizer.out.log</string>

    <key>StandardErrorPath</key>
    <string>/Users/yourusername/logs/fileorganizer.err.log</string>

    <key>ProgramArguments</key>
    <array>
        <string>/Library/Frameworks/Python.framework/Versions/3.14/bin/python3</string>
        <string>/Users/yourusername/path/to/File_Organizer.py</string>
    </array>

    <key>RunAtLoad</key>
    <true/>

    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>9</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>

</dict>
</plist>
```

3. Load the agent:
```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.yourname.fileorganizer.plist
```

4. Verify it registered:
```bash
launchctl list | grep fileorganizer
```

---

## Configuration

To change the source directory, update this line in the script:

```python
Source_Directory = Path('/Users/yourusername/Downloads')
```

To add new file types, add them to the `list_of_directories` dictionary inside the `directories()` function:

```python
"eBooks": [".epub", ".mobi"]
```

---

## Troubleshooting

If the LaunchAgent runs but files aren't moving, check the error log:

```bash
cat ~/logs/fileorganizer.err.log
```
