# Repo Updater

A  bash script to automate updating multiple Git repositories and their branches simultaneously.

## Features
- Updates multiple repositories in one go.
- Fast-forwards remote branches without needing to check them out.
- Interactive protection against losing uncommitted changes.
- Safe directory navigation and rich statistics tracking.

## Usage

1. In the folder containing the projects you want to update, copy the example config file:
   ```bash
   cp ~/project/Scripts/repo-updater/update.conf-example ./update.conf
   ```
2. Edit `./update.conf` with your specific repository folder names and GitHub username.
3. Run the script from that directory (assuming you symlinked the script to your `~/.local/bin/` folder as explained in the main repository README):
   ```bash
   update
   ```
