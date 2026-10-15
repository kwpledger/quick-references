---
name: analyze-video
description: Interactive, step-by-step review of a video's presentation (visuals, aesthetics, and consistency with the narration) using yt-dlp and ffmpeg on Kevin's Windows computer, with frames read back as labeled contact sheets. Use when Kevin types /analyze-video or asks to start the analyze-video skill, or asks to review a YouTube video or one of his own videos for visuals, consistency, or pacing. Run one step at a time and wait for Kevin's confirmation before the next step.
---

# Analyze video (interactive)

Walk Kevin through the review one step at a time. Do not give the next step until he says he has finished the current one. Keep each message short: the instruction, the command block if there is one, and a reminder to reply "Done" when finished.

Use AskUserQuestion for choices. Use plain text for anything that needs a typed answer, such as a URL.

## Fixed settings

- Terminal: PowerShell. Use PowerShell syntax in every command block.
- Root folder: `D:\Claude Cowork Projects\Video Analysis`
- Saves folder: `D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves`. This holds the downloaded MP4 and VTT, named by the video's title.
- Project folder: `D:\Claude Cowork Projects\Video Analysis\STEM`, where STEM is the saved file name without its extension, for example `Geometric Mean Annual Rate of Return (YouTube v-1uYe_w_rcvk)`. It holds `frames\`, `sheets\`, the transcript (`.en.vtt`), and `writeup.md`.
- Frames: one per second, in `STEM\frames\`. `f_00001.jpg` is second 0, `f_00002.jpg` is second 1, and so on.
- Contact sheets: 20 frames each (5 across, 4 down), in `STEM\sheets\`. `sheet_001.jpg` covers seconds 0 to 19, `sheet_002.jpg` covers seconds 20 to 39, and so on. Tell Kevin the range for each sheet when you read it.
- Quote every Windows path, because folder names contain spaces and parentheses.
- Never type the STEM by hand. Take it from the name yt-dlp reports in Step 2, and check it against the file listing in Step 4.

## Step 0: Start

Before asking anything, check that the device link can read `D:\Claude Cowork Projects\Video Analysis`. If it can't, stop and tell Kevin that Filesystem access to that folder isn't active in this session, and that a new session may be needed after the permission change. Do not continue until the folder is readable.

Then ask with AskUserQuestion:

1. **Video source?** Options: YouTube video, One of my own videos.

If it is one of his own videos, skip Steps 1 to 3. Ask him to put the MP4 in the saves folder, and ask for its exact file name. Then go to Step 4.

Then say: "Paste the video's URL."

## Step 1: Confirm the video ID

From the URL, the ID is the part after `v=` (for example, `https://www.youtube.com/watch?v=ABC123` gives `ABC123`). Confirm the ID back to Kevin before continuing.

## Step 2: Get the exact file name from yt-dlp

Tell Kevin to run this in PowerShell. It asks yt-dlp for the file name without downloading anything:

```
yt-dlp --js-runtimes node --skip-download --windows-filenames --print filename -o "%(title)s (YouTube v-%(id)s).%(ext)s" "VIDEO_URL"
```

Replace `VIDEO_URL` with the URL. Ask him to paste the printed line. Drop the extension to get STEM. Use STEM exactly as printed for every later step, and do not reword the title.

## Step 3: Download the video and captions

Tell Kevin to run these two commands in PowerShell:

```
yt-dlp --js-runtimes node -f "bv*[height<=1080]+ba/b" --merge-output-format mp4 --windows-filenames -o "D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves\%(title)s (YouTube v-%(id)s).%(ext)s" "VIDEO_URL"
yt-dlp --js-runtimes node --skip-download --write-auto-subs --sub-langs en --windows-filenames -o "D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves\%(title)s (YouTube v-%(id)s).%(ext)s" "VIDEO_URL"
```

The `--js-runtimes node` flag is required on this machine. Without it, yt-dlp warns that no JavaScript runtime was found and may drop formats. If that warning returns, ask Kevin to run `node --version` to confirm Node.js is installed. The `--merge-output-format mp4` flag asks for an MP4, so Step 4 has a predictable input.

Ask him to reply "Done" when both finish, and to paste any error text. Do not guess a fix.

## Step 4: Confirm the saved files

Tell Kevin to run this and paste the output:

```
dir "D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves"
```

Check the listing for `STEM.mp4`. The MP4 must have that exact name. If the extension is `.webm` or the name differs from Step 2, stop and show Kevin the difference. Do not continue until the names match. For his own videos, check that the file he named is there.

## Step 5: Make the project folders and move the transcript

Tell Kevin to run these commands. The project folder is named from STEM, so it matches the video file. Moving the VTT keeps the transcript with the analysis:

```
New-Item -ItemType Directory -Force -Path "D:\Claude Cowork Projects\Video Analysis\STEM\frames"
New-Item -ItemType Directory -Force -Path "D:\Claude Cowork Projects\Video Analysis\STEM\sheets"
Move-Item "D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves\STEM.en.vtt" "D:\Claude Cowork Projects\Video Analysis\STEM\"
```

If the VTT is missing from the saves folder because it was already moved, that's fine. Confirm it is in the project folder instead. Wait for "Done."

## Step 6: Extract one frame per second

```
ffmpeg -i "D:\Claude Cowork Projects\Video Analysis\_yt-dlp_saves\STEM.mp4" -vf fps=1 "D:\Claude Cowork Projects\Video Analysis\STEM\frames\f_%05d.jpg"
```

Wait for "Done." If ffmpeg reports an error, ask Kevin to paste it.

## Step 7: Build the contact sheets

```
ffmpeg -i "D:\Claude Cowork Projects\Video Analysis\STEM\frames\f_%05d.jpg" -vf "scale=320:-1,tile=5x4" "D:\Claude Cowork Projects\Video Analysis\STEM\sheets\sheet_%03d.jpg"
```

Wait for "Done." Then ask Kevin to read the `sheets` folder listing back, or read it through the device link. Tell him how many sheets there are and how much video that covers (each sheet is 20 seconds).

## Step 8: Read the sheets

Read each sheet through the device link (`mcp__remote-devices__Filesystem__read_media_file` on `D:\Claude Cowork Projects\Video Analysis\STEM\sheets\sheet_001.jpg`). Read the transcript from `D:\Claude Cowork Projects\Video Analysis\STEM\STEM.en.vtt` with `read_text_file`. Start with `sheet_001.jpg`. For each sheet:

- Note the time range it covers.
- Note the layout, text on screen, palette, and type.
- Note what the narration says for the same range, using the transcript.

Report each sheet's findings to Kevin before moving to the next sheet. Ask whether to continue after every few sheets, so he can stop early.

## Step 9: Write up the findings

Once all sheets are reviewed, produce:

1. **Summary**: 3 to 5 sentences.
2. **Timeline**: one line per sheet, with the time range, what is said, and what is shown.
3. **Findings**, grouped as Consistency (visuals against narration), Aesthetics (palette, type, contrast, readability), and Pacing. Each finding has a timestamp and a one-line fix.
4. **Not verified**: anything the sheets could not show. Contact sheets are 320 pixels wide, so small on-screen text may be unreadable. Say so when it matters.

Save the write-up as `D:\Claude Cowork Projects\Video Analysis\STEM\writeup.md`. If the device link can't write there, give Kevin the text and ask him to save it into the project folder.

If the video is Kevin's own, check the findings against his brand voice rules, and use the `kevin-brand-voice` skill for copy changes.

## Rules for the whole session

- One step at a time. Do not list future steps in advance.
- If a command fails, ask Kevin to paste the error. Do not guess a fix.
- Do not run a step until the STEM and file names from Step 4 match.
- If Kevin asks to skip a step, skip it and say what that costs (for example, skipping the captions means no transcript check).
- Never claim to have seen a frame or a sheet that was not read.
- Contact sheets are 320 pixels wide. Fine detail, such as graph paper, small print, faint diagrams, or low-contrast content, can look blank. Before calling any range blank or text-free, pull full-size frames from that range and check again.
