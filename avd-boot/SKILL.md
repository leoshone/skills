---
name: avd-boot
description: 在 Windows 上启动本机 Android 模拟器（AVD）的正确方法——必须用计划任务脱离调用会话启动，否则模拟器会随 CLI/工具调用结束被杀。触发词：启动模拟器、启动AVD、launch emulator、模拟器起不来。
agent_created: true
---

# 启动 Android 模拟器（AVD）

## AVD 位置

常用 AVD 在 `D:\Software\Android\AVD\`（`TestPhone`），启动脚本为同目录 `AVD_boot.vbs`。

## 启动（计划任务方式）

从 CLI / AI 工具调用里启动，必须用计划任务让进程脱离调用会话：

```powershell
Register-ScheduledTask -TaskName "emu_test" -Action (New-ScheduledTaskAction `
  -Execute 'D:\Software\Android\SDK\emulator\emulator.exe' `
  -Argument '-avd TestPhone -gpu swiftshader_indirect') `
  -Trigger (New-ScheduledTaskTrigger -Once -At (Get-Date).AddSeconds(5))
Start-ScheduledTask -TaskName "emu_test"
```

日常手动启动直接双击 `D:\Software\Android\AVD\AVD_boot.vbs`。

## 确认就绪

```bash
ADB="D:/Software/Android/SDK/platform-tools/adb.exe"
"$ADB" start-server
# 轮询 boot_completed（一般 25~40s），看到 1 即就绪
"$ADB" -s emulator-5554 shell getprop sys.boot_completed
"$ADB" devices        # emulator-5554  状态须是 device 而非 offline
```

把"起 adb server → 启动模拟器 → 轮询 → 业务操作"放在**同一次调用**里做完；每次新调用开头先 `adb start-server`（adb server 重启会让模拟器掉线）。

## CDP 接入（浏览器自动化用）

```bash
"$ADB" -s emulator-5554 forward tcp:9222 localabstract:chrome_devtools_remote
curl -s --noproxy "*" http://127.0.0.1:9222/json/version
```

forward 每轮 shell 都会丢，用前重建。
