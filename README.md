Почитать описание на русском языке можно [здесь](README.ru.md)
# Windows 11 Language-Theme Switcher 🔄🎨

*Developed with the assistance of Google Gemini AI.*

A lightweight automation script for **Windows 11 (24H2 / 26H2+)** that synchronizes the operating system's visual theme (Light/Dark) with your current keyboard layout.

The script operates at a low system level, instantly responding to layout changes without putting any measurable load on the CPU, regardless of which application you are currently using.

<img width="800" height="449" alt="lt" src="https://github.com/user-attachments/assets/c35d68fa-d211-416d-9b44-46397c676788" />

## 📥 How to Download

You don't need to copy the code manually to download the script:

1. Click the green **Code** button in the top right corner of this page.
2. Select **Download ZIP** from the dropdown menu.
3. Extract the downloaded archive to a convenient location on your computer.

## 📦 Installation & Autostart Guide

1. Extract the `StartSwitcher.cmd` and `EnRuSwitcher.ps1` files into the same folder.
2. Right-click on the **StartSwitcher.cmd** file and select **Create shortcut**.
3. Press **Win + R**, type `shell:startup`, and hit **Enter** (this opens the Windows Startup folder).
4. Move the newly created shortcut into this folder.
5. Run the script by double-clicking the shortcut.

*The script will now run in the background and will automatically launch every time you boot your PC.*

## 🖥 System Requirements

* **OS:** Windows 11 (Tested on versions 24H2 and 26H2)
* **Interpreter:** **PowerShell 7.6.7+** (External stable version; the built-in Windows PowerShell 5.1 is not supported).

> [!WARNING]
> This script is an amateur development and has not been tested on a wide range of PC configurations. For safety reasons, **make sure to create a System Restore Point** before running it for the first time:
> Press **Win + R** ➔ type `systempropertiesprotection` ➔ click the **"Create..."** button.

📌 **Operation & Synchronization Details:**
The script is designed as an ultra-lightweight trigger. To ensure maximum performance and zero CPU overhead, it does not constantly monitor the active Windows language API. Instead, it listens exclusively for the **Left Shift + Left Alt** key combination and inverts the current theme state (0 ➔ 1 / 1 ➔ 0).

*Important Caveat:* Since the script does not actively "know" which language is currently active in the system, the initial theme state upon PC boot depends entirely on the mode you were in when you shut down the computer.

*Recommendation:* To ensure the OS theme matches your default language on the next startup, switch to that language before shutting down your PC or locking the screen (this allows the trigger to reset the theme). Additionally, you can map a light or dark theme to a specific language simply by switching languages in the system tray.

## ⚙ Technical Overview (How it works)

The script's logic is fully open-source and operates in two stages:

1. **Global Keyboard Hook (`SetWindowsHookEx`):** The script intercepts the **Left Shift + Left Alt** key combination at a low level. It does not interfere with Windows changing the input language itself.
2. **Registry Inversion:** Once the keypress is captured, the script instantly accesses the registry branch `HKCU\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize` and toggles the `AppsUseLightTheme` and `SystemUsesLightTheme` flags to their opposite values (from 0 to 1 or vice versa). Finally, it sends a broadcast signal via `SendMessageTimeout` to force all open windows to refresh their UI instantly.
