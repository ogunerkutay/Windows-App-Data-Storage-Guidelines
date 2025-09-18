# 📂 Windows App Data Storage Guidelines

A quick reference for where to store **application files** on Windows.  
Following these conventions keeps apps clean, avoids permission issues, and aligns with industry standards.

---

## 🗂 Key Directories

| Folder | Usage | Access |
|--------|-------|--------|
| **AppData\Roaming** | User-specific **settings, configs, small data** that should move with the user across domain PCs. | `%APPDATA%` → `C:\Users\<User>\AppData\Roaming` |
| **AppData\Local** | User-specific **non-roaming data**, like caches, logs, temp DBs. Stays on that PC. | `%LOCALAPPDATA%` → `C:\Users\<User>\AppData\Local` |
| **AppData\Local\Temp** | Short-lived, disposable files. OS may auto-delete. | `%TEMP%` → `C:\Users\<User>\AppData\Local\Temp` |
| **ProgramData** | Machine-wide, non-user-specific data. Shared configs, DBs, license info. | `%PROGRAMDATA%` → `C:\ProgramData` |
| **Documents** | Files visible to the user (documents, projects). | `%USERPROFILE%\Documents` |
| **Registry** | Small settings, app state, licensing keys. | `HKCU\Software\<App>` (per-user)<br>`HKLM\Software\<App>` (machine-wide) |

---

## 🔑 Rules of Thumb

- **Settings / Configs → Roaming**  
- **Cache / Logs / Big Files → Local**  
- **Shared Data → ProgramData**  
- **User Documents → Documents**  
- **Temporary Files → Temp**  
- **Small Key-Values → Registry**

❌ Never write to `Program Files` at runtime (requires admin & breaks UAC rules).

---

## 🛠 Accessing Paths in .NET (C#)

```csharp
// User-specific folders
string roaming = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
string local = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
string temp = Path.GetTempPath();

// Example usage
string settingsPath = Path.Combine(roaming, "MyApp", "settings.json");
string cacheFolder = Path.Combine(local, "MyApp", "cache");
✅ Always use Environment.SpecialFolder — don’t hardcode paths.

📌 Example App File Layout

![Layout][(image.png)]

✅ Best Practices
Respect Windows conventions (don’t confuse roaming/local).

Store only what’s necessary (avoid bloat in roaming).

Use registry only for small configs, not data storage.

Clean up temp files after use.

Use installer/project setup to create default directories.

📖 Quick mental model:

Roaming = “Settings I want everywhere”

Local = “Stuff that belongs only to this PC”

Temp = “Safe to throw away”

ProgramData = “Shared across users”

Registry = “Tiny key/value pairs”

