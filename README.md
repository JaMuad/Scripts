# Scripts

This repository contains all of my personal automation scripts and tools.

## Available Scripts

| Script Folder | Global Command | Description |
|---|---|---|
| [📁 repo-updater](./repo-updater/) | `update` | Automatically loops through multiple local repositories and fast-forwards all active branches. |
| [📁 brave-profiles](./brave-profiles/) | `brave-profile` | Interactively creates fully isolated Brave browser profiles, each with its own launcher, window name and optional custom DNS. |
| *📁 (More Coming Soon...)* | | |

Each folder has its own README with features, prerequisites, usage and troubleshooting.

## Installation Strategy

I store all of my scripts in this central repository and execute them globally by symlinking them to my `~/.local/bin/` directory. Because the commands are symlinks, a `git pull` updates every command instantly with no re-linking needed.

### Example Setup (New Machine)

When on a new machine, clone the repo once, then symlink the scripts you want so you can run them from anywhere in your terminal:

```bash
# 1. Clone the repository (adjust the path to wherever you keep your projects)
git clone git@github.com:JaMuad/Scripts.git ~/project/Scripts

# 2. Make sure your local bin directory exists
mkdir -p ~/.local/bin

# 3. Create a symlink for each script you want to use
ln -s ~/project/Scripts/repo-updater/update ~/.local/bin/update
ln -s ~/project/Scripts/brave-profiles/brave-profile ~/.local/bin/brave-profile

# 4. Make sure the original scripts are executable
chmod +x ~/project/Scripts/repo-updater/update ~/project/Scripts/brave-profiles/brave-profile
```

To see which commands you have installed and where each one points:

```bash
ls -l ~/.local/bin
```

> [!WARNING]
> **Broken Symlinks (Moving the Project)**
> Symlinks use absolute, exact paths. If you ever move this repository to a different folder (e.g. from `~/project/` to `~/Documents/`), the symlink will break and your commands will stop working (outputting a `command not found` error). If this happens, simply delete the old link and recreate it pointing to the new location:
> ```bash
> rm ~/.local/bin/update
> ln -s ~/Documents/Scripts/repo-updater/update ~/.local/bin/update
> ```

### Setting up your PATH

If you've never used `~/.local/bin/` before, your terminal needs to know to look there for commands. Check first, since many distros (including Fedora and Ubuntu) already include it:

```bash
echo "$PATH" | tr ':' '\n' | grep -x "$HOME/.local/bin"
```

If that prints the path, you're done. If it prints nothing, add it:

1. Open your shell config file. Use `~/.bashrc` for **Bash** (the default on Fedora, Ubuntu and Debian) or `~/.zshrc` for **Zsh** (the default on macOS and Kali):
   ```bash
   nano ~/.bashrc
   ```
2. Scroll to the very bottom of the file and paste this exact line:
   ```bash
   export PATH="$HOME/.local/bin:$PATH"
   ```
3. Save the file (if using nano, press `Ctrl+O`, then `Enter`, then `Ctrl+X` to exit).
4. Reload your terminal so the changes take effect immediately:
   ```bash
   source ~/.bashrc
   ```

### Executing your scripts

Once your PATH is set up and the script is symlinked, you can execute it from **any folder** simply by typing its name!

For example:
```bash
update           # run the repo updater
brave-profile    # create a new isolated Brave profile
```

## Best Practices
- **Do not commit actual `.conf` files.** Always use `.conf-example` templates and keep your local, personal configurations excluded from Git.
- Add `*.conf` to a global `.gitignore` or this repository's `.gitignore` to prevent accidentally uploading sensitive paths or usernames.
- **Run `chmod +x` before committing a new script.** Git stores the executable flag, and downloaded files usually lose it.
- **Keep scripts free of personal data.** Use `$HOME` instead of hard-coded paths, and generic examples instead of real account or profile names.
