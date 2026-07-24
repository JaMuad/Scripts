# Scripts

This repository contains all of my personal automation scripts and tools.

## Installation Strategy

I store all of my scripts in this central repository and execute them globally by symlinking them to my `~/.local/bin/` directory.

### Example Setup (New Machine)

When on a new machine, you can symlink a script like this so you can run it from anywhere in your terminal:

```bash
# 1. Make sure your local bin directory exists
mkdir -p ~/.local/bin

# 2. Create a symlink to the script inside the repo
ln -s ~/project/Scripts/repo-updater/update ~/.local/bin/update

# 3. Make sure the original script is executable
chmod +x ~/project/Scripts/repo-updater/update
```

### Setting up your PATH

If you've never used `~/.local/bin/` before, your terminal needs to know to look there for commands.

For **Zsh** (most common):
1. Open your zsh config file in your terminal: 
   ```bash
   nano ~/.zshrc
   ```
2. Scroll to the very bottom of the file and paste this exact line:
   ```bash
   export PATH="$HOME/.local/bin:$PATH"
   ```
3. Save the file (if using nano, press `Ctrl+O`, then `Enter`, then `Ctrl+X` to exit).
4. Reload your terminal so the changes take effect immediately:
   ```bash
   source ~/.zshrc
   ```

For **Bash**:
*(Follow the exact same steps, but use `~/.bashrc` instead).*

## Best Practices
- **Do not commit actual `.conf` files.** Always use `.conf-example` templates and keep your local, personal configurations excluded from Git.
- Add `*.conf` to a global `.gitignore` or this repository's `.gitignore` to prevent accidentally uploading sensitive paths or usernames.
