# multigravity-pro

[![GitHub Repository](https://img.shields.io/badge/GitHub-Repository-blue?logo=github)](https://github.com/shubashbiswas/multigravity-pro)
[![GitHub Profile](https://img.shields.io/badge/GitHub-Profile-blue?logo=github)](https://github.com/shubashbiswas)
![Stars](https://img.shields.io/github/stars/shubashbiswas/multigravity-pro?style=social)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

<img src="assets/multigravity-logo.jpg" alt="Multigravity" width="80">

## Multigravity Pro

**Run multiple Antigravity IDE profiles simultaneously — each with its own accounts, settings, keystrokes, and extensions.**

No more logging in and logging out. Switch profiles instantly from your terminal or launch multiple profiles side-by-side at the same time.

> **Supported Operating Systems:** macOS, Windows (PowerShell 5.1 & Core 7+), and Linux.  
> **Antigravity Compatibility:** Fully supports Antigravity IDE 2.0 (`Antigravity-IDE.exe` / `Antigravity IDE.app`) as well as classic Antigravity releases.

---

## Table of Contents

- [Features](#features)
- [Installation](#installation)
  - [macOS / Linux](#macos--linux)
  - [Windows](#windows)
  - [Manual Installation](#manual-installation)
  - [Verifying Installation](#verifying-installation)
- [Getting Started](#getting-started)
  - [1. Create a Profile](#1-create-a-profile)
  - [2. Launch a Profile](#2-launch-a-profile)
  - [3. Enable Shell Autocompletion](#3-enable-shell-autocompletion)
- [Profile Types: Full vs. Auth-Only](#profile-types-full-vs-auth-only)
- [Templates](#templates)
- [Status & Process Monitoring](#status--process-monitoring)
- [Storage & Stats](#storage--stats)
- [Export & Import](#export--import)
- [Profile Management (Clone, Rename, Delete)](#profile-management-clone-rename-delete)
- [Environment Health (Doctor)](#environment-health-doctor)
- [Updating Multigravity](#updating-multigravity)
- [Uninstallation](#uninstallation)
- [All Commands Reference](#all-commands-reference)
- [Profile Directory Architecture](#profile-directory-architecture)
- [Environment Variables](#environment-variables)
- [Naming Rules & Validation](#naming-rules--validation)
- [Troubleshooting & FAQ](#troubleshooting--faq)
- [What's New in Pro](#whats-new-in-pro)
- [Credits & License](#credits--license)

---

## Features

- ⚡ **Simultaneous Profiles:** Run work, personal, and client environments side-by-side with complete session and credential isolation.
- 🎯 **Antigravity IDE 2.0 Ready:** Native detection of modern `Antigravity IDE` executables and dual extension directories (`.antigravity` & `.antigravity-ide`).
- 🪶 **Auth-Only Profiles:** Symlink your existing extensions and settings while keeping only login accounts separate (~2 MB vs ~500 MB).
- 📋 **Profile Templates:** Save golden profiles with pre-configured extensions and settings, then stamp out new profiles with `--from <template>`.
- 🖥️ **Native Desktop Launchers:** Automatically generates clickable macOS `.app` bundles, Windows Start Menu shortcuts, and Linux `.desktop` entries.
- 📦 **Export & Import:** Pack profiles into single archives (`.zip` on Windows, `.tar.gz` on macOS/Linux) to share setups across machines or back up your state.
- 📊 **Status & Stats Dashboards:** Inspect running processes, disk usage per profile, last used timestamps, and installed extension counts.
- 🛡️ **Cross-Platform Resilience:** Built from scratch with hardened PowerShell stream handling and robust bash POSIX compatibility.

---

## Installation

### macOS / Linux

Open your terminal and run:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/shubashbiswas/multigravity-pro/main/install.sh)"
```

The installer detects your OS, places `multigravity` into `/usr/local/bin` (or `~/.local/bin`), and configures the macOS app launcher icon if on Darwin.

### Windows

Open **PowerShell** (Windows PowerShell 5.1 or PowerShell Core 7+) and run:

```powershell
irm https://raw.githubusercontent.com/shubashbiswas/multigravity-pro/main/install.ps1 | iex
```

The script installs `multigravity.ps1` and a `multigravity.cmd` wrapper into `$env:USERPROFILE\.local\bin`, automatically adding it to your User `PATH`.

### Manual Installation

If you prefer manual setup:

1. Clone or download this repository:
   ```bash
   git clone https://github.com/shubashbiswas/multigravity-pro.git
   ```
2. **On macOS / Linux:**
   ```bash
   cp multigravity-pro/multigravity /usr/local/bin/multigravity
   chmod +x /usr/local/bin/multigravity
   ```
3. **On Windows:**
   - Copy `multigravity.ps1` to a directory on your `PATH` (e.g., `%USERPROFILE%\.local\bin`).
   - Create a `multigravity.cmd` helper or invoke PowerShell directly.

### Verifying Installation

Verify that Multigravity can locate Antigravity and that your directory permissions are healthy:

```bash
multigravity doctor
```

Output:
```text
Checking multigravity environment...
  [OK] Antigravity: Found at C:\Users\username\AppData\Local\Programs\Antigravity IDE\Antigravity-IDE.exe
  [OK] Global Binary: C:\Users\username\.local\bin\multigravity.cmd
  [OK] Profile storage: C:\Users\username\AntigravityProfiles (writable)

Your environment looks perfect!
```

---

## Getting Started

### 1. Create a Profile

Create an isolated profile named after a project, client, or workflow:

```bash
multigravity new work
multigravity new personal
multigravity new client-alpha
```

When created, Multigravity configures the sandbox directory structure and creates a clickable desktop launcher:
- **macOS:** `~/Applications/Multigravity <name>.app`
- **Windows:** Start Menu shortcut (`Multigravity <name>`)
- **Linux:** `~/.local/share/applications/multigravity-<name>.desktop`

### 2. Launch a Profile

Launch any profile by passing its name:

```bash
multigravity work
```

Antigravity opens in its own window using that profile's sandboxed accounts, settings, and extensions.

#### Passing Arguments to Antigravity

Any extra arguments after the profile name are forwarded directly to the Antigravity binary:

```bash
# Open current working directory
multigravity work .

# Open a specific project folder in a new window
multigravity work ~/projects/my-app --new-window

# Open a specific file
multigravity personal src/index.ts
```

### 3. Enable Shell Autocompletion

Enable tab autocompletion for profile names and commands:

#### On Windows (PowerShell)
Run:
```powershell
multigravity completion
```
Then add the following line to your PowerShell `$PROFILE` (e.g. `notepad $PROFILE`):
```powershell
Invoke-Expression (& multigravity completion powershell)
```
Reload your profile with `. $PROFILE`.

#### On macOS / Linux (bash / zsh)
Run:
```bash
multigravity completion
```
Follow the printed instructions to add completion to `~/.bashrc` or `~/.zshrc`. You can also generate the script directly:
```bash
# For Zsh
multigravity completion zsh > ~/.zsh/completion/_multigravity

# For Bash
multigravity completion bash > ~/.bash_completion.d/multigravity
```

---

## Profile Types: Full vs. Auth-Only

Most developers only need separate accounts (e.g., separate Google or work logins) while retaining identical extensions, themes, and keybindings. **Auth-only profiles** solve this with symlinks.

```bash
# Create an auth-only profile
multigravity new client-a --auth-only
```

### Comparison Matrix

| Feature | Full Profile (`multigravity new <name>`) | Auth-Only Profile (`--auth-only`) |
|---|---|---|
| **Account / Login State** | Completely isolated | Completely isolated |
| **Extensions** | Separate independent copy | Shared via symlink with default install |
| **Settings & Keybindings** | Separate independent copy | Shared via symlink with default install |
| **Disk Storage** | ~500 MB+ per profile | ~1 – 2 MB per profile |
| **Best For** | Completely distinct dev stacks (e.g., Python vs. Rust vs. Mobile) | Multiple accounts with identical extensions & editor settings |

> [!NOTE]
> On Windows, creating symlinks for auth-only profiles works best with **Windows Developer Mode** enabled, or when running in an elevated shell.

---

## Templates

Configure a base environment once (extensions, themes, keybindings, settings) and stamp out identical copies on demand:

```bash
# 1. Save an existing profile as a reusable template
multigravity template save work python-dev

# 2. View all saved templates and their disk footprint
multigravity template list

# 3. Create a new profile instantly from the template
multigravity new client-apollo --from python-dev

# 4. Delete a template when no longer needed
multigravity template delete python-dev
```

---

## Status & Process Monitoring

Check which profiles are currently running, their profile type, last accessed time, and disk size:

```bash
multigravity status
```

Example terminal output:
```text
PROFILE          STATUS     TYPE         LAST USED            SIZE
-------          ------     ----         ---------            ----
client-a         running    auth-only    2026-03-10 14:30     2 MB
client-b         stopped    auth-only    2026-03-08 09:15     1 MB
personal         stopped    full         2026-03-09 20:00     487 MB
work             running    full         2026-03-10 14:32     523 MB
```

---

## Storage & Stats

Inspect the exact disk usage and installed extension count across all profiles:

```bash
multigravity stats
```

Example output:
```text
Profile Storage Usage:
PROFILE              SIZE       EXTENSIONS
-------              ----       ----------
work                 512.4 MB   18
personal             480.2 MB   14
client-a             2.1 MB     0 (shared)

Total usage: 994.7 MB
```

---

## Export & Import

Move complete profiles between computers, share standard team environments, or back up critical setups.

### Exporting a Profile

```bash
# On macOS / Linux (creates a .tar.gz archive)
multigravity export work ~/Desktop/work.tar.gz

# On Windows (creates a .zip archive)
multigravity export work C:\backup\work.zip

# Default path: outputs ./<name>.zip or ./<name>.tar.gz if destination omitted
multigravity export work
```

### Importing a Profile

```bash
# Auto-detects profile name from file name ('work' in this case)
multigravity import work.tar.gz

# Import on Windows with a custom profile name
multigravity import C:\backup\work.zip client-new
```

When imported, Multigravity extracts the data into your profile store and immediately registers native desktop and Start Menu shortcuts.

---

## Profile Management (Clone, Rename, Delete)

### Clone a Profile
Duplicate an existing profile, including all installed extensions and configuration:
```bash
multigravity clone work work-backup
```

### Rename a Profile
Rename a profile folder and automatically update or re-link all OS shortcuts:
```bash
multigravity rename work-backup work-v2
```

### Delete a Profile
Delete a profile and purge its data directory. Prompts for confirmation before proceeding:
```bash
multigravity delete test-profile
```
> [!WARNING]
> Ensure Antigravity is closed for that profile before deleting to avoid locked file errors.

---

## Environment Health (Doctor)

Run `multigravity doctor` anytime you encounter issues or after updating your Antigravity installation:

```bash
multigravity doctor
```

The doctor verifies:
1. **Antigravity Executable:** Detects `Antigravity-IDE.exe`, `Antigravity IDE.exe`, `Antigravity.app`, or PATH binaries.
2. **Global Command:** Validates that `multigravity` is located in your system PATH.
3. **Storage Directory:** Confirms read and write permissions in the profiles home directory.

---

## Updating Multigravity

Upgrade Multigravity to the latest release directly from GitHub:

```bash
multigravity update
```

The CLI downloads the latest release script and replaces the installed executable in place.

---

## Uninstallation

If you ever need to uninstall Multigravity:

### On Windows
1. Delete the installed scripts:
   ```powershell
   Remove-Item "$env:USERPROFILE\.local\bin\multigravity.*" -Force
   ```
2. (Optional) Remove your profiles and templates data:
   ```powershell
   Remove-Item -Recurse -Force "$env:USERPROFILE\AntigravityProfiles"
   ```
3. Remove Start Menu shortcuts:
   ```powershell
   Get-ChildItem "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Multigravity *.lnk" | Remove-Item -Force
   ```

### On macOS / Linux
1. Remove the executable and macOS icon:
   ```bash
   sudo rm -f /usr/local/bin/multigravity /usr/local/bin/icon.icns
   # Or if installed in ~/.local/bin:
   rm -f ~/.local/bin/multigravity ~/.local/bin/icon.icns
   ```
2. (Optional) Remove profiles data:
   ```bash
   rm -rf ~/AntigravityProfiles
   ```
3. Remove launchers:
   - macOS: `rm -rf ~/Applications/Multigravity\ *.app`
   - Linux: `rm -f ~/.local/share/applications/multigravity-*.desktop ~/.local/share/multigravity/launchers/*.sh`

---

## All Commands Reference

| Command | Arguments | Description |
|---|---|---|
| `multigravity new` | `<name> [--auth-only] [--from <template>]` | Create a new profile and generate OS shortcuts. |
| `multigravity <name>` | `[args...]` | Launch Antigravity with the specified profile. Extra args forwarded. |
| `multigravity list` | `[--raw]` | List all existing profile names. |
| `multigravity status` | — | Display running status, profile type, last accessed time, and disk size. |
| `multigravity clone` | `<src> <dest>` | Duplicate an existing profile into a new one. |
| `multigravity rename` | `<old> <new>` | Rename a profile and refresh desktop launchers. |
| `multigravity delete` | `<name>` | Safely delete a profile and its shortcuts (confirms first). |
| `multigravity template save` | `<profile> <name>` | Save a configured profile as a reusable blueprint. |
| `multigravity template list` | — | List all saved templates and their disk usage. |
| `multigravity template delete`| `<name>` | Delete a saved template. |
| `multigravity export` | `<name> [path]` | Export a profile to a `.tar.gz` (macOS/Linux) or `.zip` (Windows) archive. |
| `multigravity import` | `<archive> [name]` | Import a profile archive with automatic launcher registration. |
| `multigravity doctor` | — | Run environment diagnosis and inspect PATH / binary health. |
| `multigravity stats` | — | Display storage breakdown and installed extension count per profile. |
| `multigravity update` | — | Download and install the latest version of Multigravity. |
| `multigravity completion` | `[bash \| zsh \| powershell]` | Generate or view shell autocompletion configuration. |
| `multigravity help` | — | Show command line help and usage summary. |

---

## Profile Directory Architecture

### Storage Locations by OS

Default base path: `~/AntigravityProfiles/<name>` (or `%USERPROFILE%\AntigravityProfiles\<name>`).

#### Windows
- **Profile Folder:** `%USERPROFILE%\AntigravityProfiles\<name>`
- **User Data:** `%USERPROFILE%\AntigravityProfiles\<name>\AppData\Roaming\Antigravity` & `Antigravity IDE`
- **Extensions:**
  - `%USERPROFILE%\AntigravityProfiles\<name>\.antigravity\extensions`
  - `%USERPROFILE%\AntigravityProfiles\<name>\.antigravity-ide\extensions` (Antigravity IDE 2.0)
- **Start Menu Shortcut:** `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Multigravity <name>.lnk`

#### macOS
- **Profile Folder:** `~/AntigravityProfiles/<name>`
- **User Data:** `~/AntigravityProfiles/<name>/Library/Application Support/Antigravity`
- **Keychains Link:** Symlinked to `~/Library/Keychains` for seamless credential management
- **Extensions:** `~/AntigravityProfiles/<name>/.antigravity/extensions`
- **Launcher:** `~/Applications/Multigravity <name>.app`

#### Linux
- **Profile Folder:** `~/AntigravityProfiles/<name>`
- **User Data:** `~/AntigravityProfiles/<name>/.config/Antigravity`
- **Cache & State:** `~/.cache`, `~/.local/share`, `~/.local/state`
- **Extensions:** `~/AntigravityProfiles/<name>/.antigravity/extensions`
- **Desktop Entry:** `~/.local/share/applications/multigravity-<name>.desktop`

---

## Environment Variables

You can customize Multigravity by exporting or setting these environment variables:

| Variable | Description | Default |
|---|---|---|
| `MULTIGRAVITY_HOME` | Directory where all profile sandboxes and templates are stored. | `~/AntigravityProfiles` or `%USERPROFILE%\AntigravityProfiles` |
| `MULTIGRAVITY_APP` | Custom path to the Antigravity or Antigravity IDE executable/app bundle. | Auto-detected standard paths |
| `MULTIGRAVITY_PLATFORM`| Force platform type (`darwin`, `linux`). Useful for scripts and testing. | Auto-detected from `uname -s` |

### Setting Environment Variables

#### Windows (PowerShell)
```powershell
# Set for current session
$env:MULTIGRAVITY_HOME = "D:\AntigravityProfiles"

# Set persistently
[Environment]::SetEnvironmentVariable("MULTIGRAVITY_HOME", "D:\AntigravityProfiles", "User")
```

#### macOS / Linux
```bash
# Add to ~/.zshrc or ~/.bashrc
export MULTIGRAVITY_HOME="/Volumes/ExternalSSD/AntigravityProfiles"
```

---

## Naming Rules & Validation

Profile and template names must strictly conform to:
- Alphanumeric characters (`a-z`, `A-Z`, `0-9`) and hyphens (`-`).
- Must **start** with an alphanumeric character.
- Spaces, underscores, and special characters are disallowed to guarantee compatibility across shells and file systems.

| Valid Names | Invalid Names | Reason |
|---|---|---|
| `work` | `-work` | Cannot start with a hyphen |
| `client-apollo` | `client_apollo` | Underscores not allowed |
| `dev-2` | `my profile` | Spaces not allowed |
| `test1` | `project@v1` | Special characters not allowed |

---

## Troubleshooting & FAQ

### 1. "Antigravity was not found" or doctor error
If Antigravity is installed in a non-standard location or portable folder, specify its location via `MULTIGRAVITY_APP`:
```powershell
# Windows
$env:MULTIGRAVITY_APP = "C:\CustomPath\Antigravity-IDE.exe"
```
```bash
# macOS
export MULTIGRAVITY_APP="/Applications/Custom/Antigravity.app"

# Linux
export MULTIGRAVITY_APP="/opt/antigravity/antigravity"
```

### 2. Can I run multiple profiles at the same time?
**Yes!** That is the core purpose of Multigravity. You can run `multigravity work` and `multigravity personal` concurrently. Each instance runs in a detached process with independent user data directories.

### 3. How do Auth-Only profiles save disk space?
Standard profiles duplicate all extensions (~30–50 MB each). Auth-only profiles create symbolic links pointing to your root extensions and settings, so only authentication credentials and session tokens occupy disk space (~2 MB).

### 4. How do I pass flags like opening in full screen or disabling extensions?
Pass flags directly after the profile name:
```bash
multigravity work --disable-extensions
multigravity personal --new-window .
```

---

## What's New in Pro

| Feature | Command | What it does |
|---|---|---|
| **Antigravity IDE 2.0 Support** | Automatic | Detects `Antigravity-IDE.exe`, modern app paths, and dual `.antigravity-ide` extension folders. |
| **Auth-Only Profiles** | `new <name> --auth-only` | Shares extensions and settings across profiles via symlinks; isolates credentials. Near-zero disk usage. |
| **Profile Templates** | `template save <prof> <tpl>` | Save configured profiles as templates; stamp out copies via `new <name> --from <tpl>`. |
| **Status Dashboard** | `status` | Live overview of running/stopped instances, profile types, last active times, and disk footprints. |
| **Storage Diagnostics** | `stats` | Profile storage breakdown and extension count monitoring. |
| **Export & Import** | `export` / `import` | Pack profiles into portable archives (`.zip` / `.tar.gz`) for migration and team sharing. |
| **Robust Windows Engine** | All commands | Rewritten PowerShell engine with proper stream piping, Start Menu shortcut management, and 40+ edge-case fixes. |

All classic commands from the original [multigravity-cli](https://github.com/sujitagarwal/multigravity-cli) remain fully compatible.

---

## Credits & License

- Built on top of [multigravity-cli](https://github.com/sujitagarwal/multigravity-cli) by [Sujit Agarwal](https://github.com/sujitagarwal).
- Maintained by [Shubash Biswas](https://github.com/shubashbiswas).

Released under the [MIT License](LICENSE).
