# Brave Profiles

An interactive bash script that creates fully isolated Brave browser profiles, each with its own launcher, window name and optional custom DNS.

## Why This Script Exists

I keep different parts of my life in separate browsers so their logins, history and cookies never mix. For example:

| Profile | What it's for |
|---|---|
| **Personal** | Day-to-day browsing (the normal Brave install) |
| **University** | Campus portals and coursework only |
| **Work** | Company accounts |
| **Work Testing** | A second work login to see the product from another user's view |
| **Social Media** | Brand or creator accounts |
| **WhatsApp** | One profile per phone number (WhatsApp Web allows one account per browser) |
| **Streaming** | Uses its own DNS (e.g. Cloudflare) while every other profile uses the normal network |
| **Presentations** | Clean browser for screen sharing, never logged in |
| **Private** | Wiped completely every time it closes |

## Quick Context

Brave's built-in **Add Profile** button (`--profile-directory`) keeps accounts apart, but every profile still runs inside **one** browser. They share one process, one data folder and browser-wide settings such as Secure DNS, so one profile can't have its own DNS.

This script gives each profile its **own data folder** (`--user-data-dir`), which makes it a completely separate browser. Nothing is shared between them: logins, cookies, history, cache, extensions, saved passwords and DNS settings all stay inside that profile.

> [!WARNING]
> Once you use this script, stop using Brave's own **Add Profile** button. One profile = one `brave-profile`.

## Features
- Asks three questions per profile (name, wipe-on-close, custom DNS) and confirms every step with ✔ / ✘ / ⚠.
- Flags duplicate names and lets you pick another name or cancel.
- Custom DNS per profile: type IPs separated by commas (e.g. `1.1.1.1,1.0.0.1`), and each server is tested live before it's accepted.
- Wipe-on-close profiles: a brand-new browser on every launch, deleted when the last window closes.
- Each profile gets its own app-menu launcher, dock icon and window name.
- `list` shows every profile with its size, type, DNS and whether it's open.
- `remove` asks before deleting anything and keeps the data unless you confirm.
- Nothing is written until you confirm the summary. Ctrl+C cancels at any point.

## Directory Structure Example

After creating a few profiles, your system will look something like this:

```text
~/.config/brave-profiles/
├── university/                 <-- Profile data (logins, history, cache)
├── work/
└── streaming/                  <-- Custom DNS saved inside this profile only

~/.local/share/applications/
├── brave-university.desktop    <-- App-menu launcher
├── brave-work.desktop
├── brave-streaming.desktop
└── brave-private.desktop       <-- Wipe-on-close launcher

/tmp/brave-private.XXXXXX/      <-- Created on launch, deleted on close
```

Your normal Brave (`~/.config/BraveSoftware/Brave-Browser/`) is not touched. It stays as your Personal profile and default browser.

## Prerequisites
- **Brave installed natively** (RPM or DEB) from [brave.com/linux](https://brave.com/linux). The Flatpak version is not supported.
- **bash 4+** and **curl** (used to test DNS servers). Both are installed by default on most distros.
- **desktop-file-utils** (optional): refreshes the app menu and validates launchers. Preinstalled on Fedora Workstation.
- `~/.local/bin` in your `PATH`. See [Setting up your PATH](../README.md#setting-up-your-path).

## Tested On
- **Fedora Workstation 44** (GNOME, Wayland)
- **Brave 1.97** (Chromium 155)

It should also work on other recent Fedora releases, Ubuntu/Debian and other distros with the official Brave package, on GNOME or KDE. Those setups are untested.

## Usage

1. Symlink the script into `~/.local/bin` (see the [Installation Strategy](../README.md#installation-strategy)):
   ```bash
   mkdir -p ~/.local/bin
   ln -s ~/project/Scripts/brave-profiles/brave-profile ~/.local/bin/brave-profile
   chmod +x ~/project/Scripts/brave-profiles/brave-profile
   ```
2. Create a profile. Run this once per profile and answer the questions:
   ```bash
   brave-profile
   ```
   ```text
   ▸ 1/3  Profile name
     Name your profile (e.g. University): University
     ✔ Name is available: "Brave University"  (id: university)

   ▸ 2/3  Profile type
     Wipe everything when this profile closes? [y/N] n
     ✔ Type: normal (keeps logins)

   ▸ 3/3  DNS
     Does this profile need its own custom DNS? [y/N] n
     ✔ Using normal network DNS.
   ```
3. Open it from your app menu: press the **Super** key and type the profile name.
4. Manage your profiles:
   ```bash
   brave-profile list      # all profiles, sizes, DNS, what's open
   brave-profile remove    # asks which one; data is kept unless you confirm
   ```

### Recommended Settings Inside Each Profile
- **Pick a different colour** (New Tab → Customize, or `brave://settings/appearance`) so you can tell windows apart at a glance.
- **Rename the internal profile** (profile icon → Edit) from "Person 1" to the profile's name.
- **Leave Brave Sync off.** Sync links profiles together again.
- **Presentation profile:** `brave://settings/clearBrowserData` → **On exit** tab → tick history, cookies and cache, so it cleans itself every time.
- **University profile:** keep normal DNS, because campus Wi-Fi login pages and internal sites need the network's own DNS.

### About Custom DNS
A browser can only use its own DNS through **encrypted DNS (DNS-over-HTTPS)**, which is why the script tests every server before accepting it. Cloudflare (`1.1.1.1,1.0.0.1`), Quad9 (`9.9.9.9,149.112.112.112`) and Google (`8.8.8.8,8.8.4.4`) all support it. You can also paste a full DoH address (`https://…/dns-query`).

Custom DNS is **strict**: if those servers can't be reached, the profile won't load pages instead of silently falling back to the network's DNS. To check it's working, open the profile and visit `https://one.one.one.one/help`.

### What Is and Isn't Isolated
- **Isolated:** logins, cookies, history, cache, extensions, saved passwords, site permissions, DNS setting.
- **Shared:**
  - **IP address.** Every profile uses the same internet connection.
  - **Linux user account.** A malicious extension could in theory read another profile's folder, so keep extensions to a minimum.
  - **Device traits** (screen, GPU, fonts).

## Troubleshooting

If the script fails to run or behaves unexpectedly, check the following common mistakes:

- **PATH Issues:** If your terminal says `command not found: brave-profile`, make sure `~/.local/bin` is in your `PATH` (`~/.bashrc` for Bash, `~/.zshrc` for Zsh).
- **Execution Permissions:** Make sure the script is executable: `chmod +x ~/project/Scripts/brave-profiles/brave-profile`.
- **"Brave isn't installed" / "Only the Flatpak version":** Install the native package from [brave.com/linux](https://brave.com/linux). The script looks for `/usr/bin/brave-browser`.
- **Profile Not in the App Menu:** Run `update-desktop-database ~/.local/share/applications`, then log out and back in.
- **All Profiles Under One Dock Icon:** On GNOME, press `Alt+F2`, type `lg`, press Enter, open the **Windows** tab and check that each window shows `brave-<id>`.
- **Window Name Not Showing:** The name only applies when that profile starts fresh. Close all of its windows, run `pgrep brave` to make sure nothing is still running in the background, then reopen it from its launcher. Alternatively, right-click the tab bar → **Name window** once; with **Continue where you left off** enabled, Brave remembers it.
- **DNS Test Fails:** The server must support DNS-over-HTTPS; plain DNS (like a router's `192.168.x.x`) can't be set per profile. If every test fails, check your internet connection. You can also choose **[k] keep anyway**.
- **Custom-DNS Profile Won't Load Pages:** The DNS servers are unreachable from your current network (strict mode). Switch the DNS off inside the profile at `brave://settings/security` → **Use secure DNS**.
- **"Is open right now" When Removing:** Close every window of that profile first.
- **Broken Symlinks (Moving the Project):** If you move the Scripts folder, recreate the symlink. Your existing profiles and launchers keep working once it points to the new location:
  ```bash
  rm ~/.local/bin/brave-profile
  ln -s ~/Documents/Scripts/brave-profiles/brave-profile ~/.local/bin/brave-profile
  ```
