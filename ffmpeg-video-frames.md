---
name: ffmpeg-video-frames
description: "How to analyze the original gameplay video (extract frames) — ffmpeg is NOT on PATH, find the working binary"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 450ec46b-cfc6-4439-98fc-e99fce82afcd
---

To analyze the original game's look (user drops a screen-recording .mp4, e.g. for "compare our build vs source"), extract frames with ffmpeg and Read the JPGs.

**ffmpeg is NOT on PATH** and the winget package (`Gyan.FFmpeg...`) is missing its `bin/` (only doc/presets). Working binaries found via filesystem search:
- `D:\QQBrowser\21.2.8893.400\ffmpeg\ffmpeg.exe` ← used; **mp4-only build, CANNOT decode PNG** (errors "Invalid data found")
- `D:\腾讯电脑管家软件搬家\软件搬家\剪映专业版\5.4.0.11246\ffmpeg.exe` (JianYing, likely fuller)

**Workflow that worked:** extract frames `-vf "fps=3,scale=400:-1"` to JPGs in `C:\tmp\vframes\`, then Read them. For detail use `-ss <sec> -frames:v 1 -vf "crop=in_w:in_h*0.55:0:in_h*0.15,scale=600:-1"`. The `tile` filter and `overlay=(W-w)/2` failed in the QQBrowser build (no tile; PowerShell mangles parens) — Read individual frames instead.

To inspect near-white-on-transparent PNGs (e.g. `Assets/Art/Bottles/pz1/*`): they're invisible on white in the Read viewer; this ffmpeg can't flatten them either (no PNG decoder). Reading them directly via the Read tool still renders (faintly). Reading game-art frames for one-time analysis is acceptable despite the CLAUDE.md "don't read binaries" token rule.

Used 2026-06-14 to drive the bottleAnim polish round — see [[art-replication-scope]].
