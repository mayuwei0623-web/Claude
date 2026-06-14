---
name: unity-batchmode-tests
description: Unity 编辑器路径与 batchmode 跑 EditMode/PlayMode 测试的命令(含 -quit 陷阱)
metadata: 
  node_type: memory
  type: reference
  originSessionId: 4d255f73-0058-49c0-95ab-14af0f2a2cbd
---

本工程用 Unity 2022.3.62f2,编辑器在 `D:\unity\2022.3.62f2\Editor\Unity.exe`(Unity Hub 副安装路径 `D:\unity`,非默认 Program Files;同目录还有多个旧版本,务必用 .62f2 匹配 ProjectSettings/ProjectVersion.txt)。

跑 EditMode 测试(PowerShell):
```
& "D:\unity\2022.3.62f2\Editor\Unity.exe" -batchmode -projectPath "D:\OverSeasGame\GameTestProject\WaterSort\Water" -runTests -testPlatform EditMode -testResults "<proj>\TestResults_EditMode.xml" -logFile "<proj>\test_editmode.log"
```

**陷阱:`-runTests` 不要和 `-quit` 一起用** —— 加了 -quit 会让 Unity 跑完不写结果 XML(日志显示 "Saving results" 但文件不生成/被提前退出截断)。去掉 -quit,batchmode 跑完测试会自动退出。结果在 testResults XML 的 `<test-run total/passed/failed/result>`。日志里 `error CS` 判编译错。

测试产物(TestResults_*.xml / *.log)不提交,commit 前删掉。当前基线:EditMode 39 用例全过。
