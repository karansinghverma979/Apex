# ⚡ Apex Workspace Rules & Architectural Invariants

> **Project**: Apex (GPU-Accelerated Virtual Desktop & Z-Order Controller)  
> **Framework**: .NET 9.0 WPF Native Windows Subsystem  
> **Author**: Karan Singh Verma

---

## 🏛️ Core Principles & Invariants

### 1. ⚡ Zero Persistent Background Daemons (0 MB Idle RAM)
- **Strictly On-Demand**: Apex exists only when summoned (`Ctrl + Alt + A`).
- **Complete Dismissal**: When dismissed via `Esc`, clicking outside, or window close (`WM_CLOSE`), the process must terminate completely.
- **No Background Services**: Never introduce persistent background polling timers, daemon loops, or hidden worker threads that consume RAM while idle.

### 2. 🚀 Sub-50ms Cold Launch & Trimming Integrity
- **High-Speed Cold Boot**: Startup latency must remain under 50ms.
- **Single-File Native Binary**: Production builds target a single-file executable (`~/.local/bin/Apex.exe`) without multi-file dependency clutter.
- **Zero Heavy Runtimes**: Avoid Electron, WebView2, or heavy multi-megabyte wrappers.

### 3. 🎯 Win32 Z-Order & Virtual Desktop Precision
- **Non-Intrusive Z-Order**: Manipulate window z-order strictly via native `SetWindowPos` (`HWND_TOPMOST` / `HWND_NOTOPMOST`) and `SWP_NOSIZE | SWP_NOMOVE | SWP_NOACTIVATE` to ensure zero focus-stealing.
- **Virtual Desktop COM Interface**: Use official Win32 `IVirtualDesktopManager` COM interfaces to inspect and transition windows cleanly across virtual desktops.
- **Process Exclusion List**: Maintain atomic, thread-safe JSON blocklist persistence in `%LOCALAPPDATA%\Apex\blacklist.json`.

### 4. 🎨 AMOLED Pitch Black Visual Language (#000000)
- **Visual Identity**: Strict pure pitch black (`#000000`) backdrop with subtle borders (`#222222`), gold accent badges (`#f59e0b`), and clean GPU-rendered typography.
- **Responsive Layout**: High-DPI scaling awareness with zero clipped borders or ragged text.

### 5. 🛣️ Zero Absolute Machine Path Invariant
- **No Hardcoded User Paths**: Never commit or compile machine-specific paths (e.g. `C:\Users\<username>\...`).
- **Dynamic Resolution**: Always resolve user directories dynamically via `Environment.GetFolderPath(Environment.SpecialFolder.UserProfile)` or `SpecialFolder.LocalApplicationData`.
