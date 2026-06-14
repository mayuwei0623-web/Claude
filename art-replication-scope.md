---
name: art-replication-scope
description: "The 美术全复刻 push — 4 confirmed goals, prefab pipeline, what's done vs remaining"
metadata: 
  node_type: memory
  type: project
  originSessionId: 07588ab7-1092-422b-980a-c346ecbe9164
---

User escalated quality bar: original UI was being rebuilt as plain code color-blocks (no extracted art), which deviated from "复用原 Cocos 美术资源". User confirmed (2026-06-14) FOUR goals for the 美术全复刻 push:
1. **UI 美术全复刻** — convert all screens to real-art prefabs.
2. **水/倒水动画复刻** — see [[spine-unity-decision]] (spine-unity approved, deferred).
3. **特效复刻** — appearEff(瓶满)/dlEff(洗瓶)/whEff(暗格).
4. **留存大厅纳入 (visual only)** — assemble lobby UI; skin/shop/sign-in functions deferred; only need 闯关 mode reachable.

User **authorized creating/modifying prefabs freely** ("UI 里的 prefab 你可以随意改").

**Pipeline:** extract sprites → `Assets/Art/UI/{Home,Game}/` (ArtTextureImporter auto-Sprite) → `Assets/Scripts/Editor/UIPrefabBuilder.cs` programmatically builds `Assets/Resources/UI/*.prefab` → runtime `UIManager.CreatePrefab<T>(path)` instantiates, `GameController` falls back to code `Create<T>` if prefab missing. `BaseUI.FromPrefab` skips code Build() when instantiated from prefab.

**Done (2026-06-14):**
1. ✅ **UI 全界面 prefab 化** (commit 28ff2c1): 7 弹窗 dual-mode (serialized refs + code Build fallback + idempotent Wire in OnOpen); `BaseUI.EditorBake` + `UIPrefabBuilder.BakePanel<T>` bakes each panel's own Build() into a prefab (no layout dup); GameController all `CreatePrefab ?? Create`. HUD real icons: undo=回退/wash=打乱/add=加空杯 on 道具按钮 frame, setting=sz_icon, top jdt01+jdt02 progress bar via `GameController.UpdateProgress` (CompletedCount/MaxScore). User decided: dialog panel art (res/ui/panel, NOT extracted) stays color-card placeholder + node contracts — 美术 drops art onto contract nodes in-editor later.
4. ✅ **留存大厅** (commit 53759e8): HomeView enriched with extracted Home deco (游戏圈/反馈/福袋/邀请/收藏/签到/排行/皮肤/无尽/我的皮肤), all raycast-off visual-only; cgms StartButton = only functional entry. Deco positions are程序估值, 美术 nudges in editor.

2. ✅ **水/倒水动画** (commits df5d0b6 stream/splash) and 3. ✅ **特效** (commit 570cb17: appearEff/dlEff/whEff): done **without spine** — see [[spine-unity-decision]] (reversed). `SplashExtractor.cs` slices real frames from the spine atlas PNGs → `Assets/Art/Game/{Splash,Stream,Wash,Appear,Reveal}/`; `SplashEffect`(flipbook)+`BurstEffect`(single-sprite) coroutines, wired via events (PourEnd/BottleComplete/BottleWashed/BottleRevealed) into a dedicated `FxContainer` (mirrors bottleContainer coords, not counted as bottles, cleared on level load).

**P2 COMPLETE — all 4 goals done, zero spine-unity / zero DOTween.** Optional future polish only: dialog-panel real art (drop onto contract nodes), additive material to brighten white effects (stream/splash/star), jp/js multi-piece full choreography (currently main-piece burst approximation).

**Polish round (2026-06-14, commit 1127932):** compared original gameplay video frame-by-frame (see [[ffmpeg-video-frames]]) vs Cocos `BottleItem.js`, fixed 5 gaps — ALL externalized to new `game_const.bottleAnim` block (`ConfigModels.BottleAnimConfig`/`SelectStyle`/`ShadowStyle`/`LiquidStyle` + validator):
1. **Liquid body color = `dark` not `light`** — DO NOT REVERT. Cocos `initWaterColor` renders body with `s[0]`=dark (saturated) and only the top surface with `s[1]`=light. We were using `light` everywhere → washed-out/pink. `colorMap[skin][id-1]` = `[dark(vivid), light(pale)]`; our config field `dark`/`light` maps to `[0]`/`[1]`. Now `BottleView.ColorForUnit` uses dark (toggle `bottleAnim.liquid.bodyUseDark`).
2. **Top liquid surface ellipse** (3D cylinder cap) — procedural soft ellipse, tinted top `light`; hidden during tilt.
3. **Select feel** (`setSelected`/`floatTw`): pickup bounce(scale 1.06)→lift 40px→continuous gentle sway (`BottleView.SelectAnim` coroutine).
4. **Shadow + selection highlight** — both reuse one procedural `SoftEllipse()` sprite (zero new art files).
5. **`pourAnim.maxTilt` 60→65** — capped because NO shader; shear distorts near 90°. Can hand-raise toward 78 (原版 tilts 78~89) but MUST eyeball in editor. Verified EditMode 39/39 + PlayMode 13/13.

**How to apply:** Full status in `docs/tasks.md`. Tunables for hand-tuning all in `game_const.bottleAnim` (documented in `docs/策划配置手册.md`). Keep tests green (now EditMode 39 + PlayMode 13); Unity 2022.3.62f2 at `D:\unity\2022.3.62f2\Editor\Unity.exe`; batch logFile needs ABSOLUTE path (relative "./x" errors out); effects must parent to FxContainer NOT bottleContainer (else inflates bottle count → test regression). Deferred (backlog): per-band inner highlight strips (need shear-coupling), pour horizontal flip (bottle art symmetric → no gain).
