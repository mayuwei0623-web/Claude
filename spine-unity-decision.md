---
name: spine-unity-decision
description: SUPERSEDED — spine-unity was NOT used; water/pour anim + effects done via extracted real frames + coroutines instead
metadata: 
  node_type: memory
  type: project
  originSessionId: 07588ab7-1092-422b-980a-c346ecbe9164
---

**SUPERSEDED (2026-06-14, same day).** The earlier plan was to import the third-party spine-unity runtime for faithful water/pour animation. That was reversed after investigation:

1. spine-unity is **not on Unity's package registry** — it ships as a manual `.unitypackage` download from esotericsoftware.com; can't be auto-installed, and the user said they can't edit the spine files (no Spine editor).
2. Inspecting the actual `.atlas` files showed these "spine" animations are **trivially simple**: `dd`(水流) and `jh`(洗瓶) are single sprites doing transform animation; `sh`(水花) is a 9-frame flipbook; `jp`(瓶满)/`js`(暗格) are a few composed pieces. **Not worth a dependency.**

**What was actually done instead** (commits df5d0b6, 570cb17): `SplashExtractor.cs` (editor) slices the real frames straight from the atlas PNGs honoring rotate/offset/orig → `Assets/Art/Game/{Splash,Stream,Wash,Appear,Reveal}/`. Runtime players `SplashEffect` (flipbook) + `BurstEffect` (single-sprite scale/fade/spin), both pure coroutines — **zero spine-unity, zero DOTween**. Wired in GameController via existing events (PourEnd/BottleComplete/BottleWashed/BottleRevealed), effects live in a dedicated `FxContainer`.

So: **DOTween and spine-unity both remain excluded from the project.** See [[art-replication-scope]] for full P2 status (all 4 goals done). Optional future polish: additive material to brighten the white effects (stream/splash/star).
