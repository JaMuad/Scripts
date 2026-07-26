# Repo Updater

A  bash script to automate updating multiple Git repositories and their branches simultaneously.

## Features
- Updates multiple repositories in one go.
- Fast-forwards remote branches without needing to check them out.
- Interactive protection against losing uncommitted changes.
- Safe directory navigation and rich statistics tracking.

## Directory Structure Example

When correctly configured, your project directory should look something like this:

```text
~/project/MyNewApp/
├── update.conf        <-- Your local configuration file
├── MyNewApp-Backend/ <-- Git Repository
└── MyNewApp-Frontend/      <-- Git Repository
```

## Usage

1. In the folder containing the projects you want to update (e.g., `~/project/MyNewApp/`), copy the example config file:
   ```bash
   cp ~/project/Scripts/repo-updater/update.conf-example ./update.conf
   ```
2. Edit `./update.conf` with your specific repository folder names and GitHub username.
3. Run the script from that directory:
   ```bash
   update
   ```

## Troubleshooting

If the script fails to run or behaves unexpectedly, check the following common mistakes:

- **Missing Configuration File:** Ensure that `update.conf` is located in the exact same directory where you are running the `update` command.
- **Missing or Empty Variables:** Double-check your `update.conf` to ensure `GITHUB_USERNAME` and `REPOS` are properly filled out and not left empty or with placeholder text.
- **Execution Permissions:** Make sure the script is executable. If it's not, run `chmod +x ~/.local/bin/update` (or point to the original file in the Scripts repo).
- **PATH Issues:** If your terminal says `command not found: update`, make sure `~/.local/bin` is added to your `PATH`. Verify whether you are using Bash (`~/.bashrc`) or Zsh (`~/.zshrc`) and that the export line was added correctly.
- **Typographical Errors:** Double-check the spelling of your folders in the `REPOS` array inside `update.conf`. They must exactly match the folder names on your local machine.
- **Broken Symlinks (Moving the Project):** Symlinks (created via `ln -s`) use exact paths. If you move this project folder to a different location (for example, from your home directory `~/` to `~/Documents/`), the symlink will break and your command will output `command not found`. To fix this, delete the broken link and create a new one pointing to the new location:
  ```bash
  rm ~/.local/bin/update
  ln -s ~/Documents/repo-updater/update ~/.local/bin/update
  ```
