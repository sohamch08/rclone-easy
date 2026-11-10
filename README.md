# Rclone Aliases

Zsh shortcuts for rclone, with formatted listings, simple transfers, and Git-style comparisons.

## Help

```bash
rchelp                 # All commands and shared rules
rchelp rcpush          # Help for a specific command
rcpush -h              # Same command help
rcpush --help          # Same command help
rc --help              # Native rclone help
```

Helper help is displayed locally without running the operation.

## Commands

### General

| Command            | What it does                                                    |
| :----------------- | :-------------------------------------------------------------- |
| `rc`               | Alias for native `rclone`.                                      |
| `rcremotes`        | List configured remotes.                                        |
| `rcconfig`         | Display config with `token` and `root_folder_id` values masked. |
| `rcedit`           | Edit config in Neovim.                                          |
| `rclog`            | Follow the rclone log.                                          |
| `rclog-clear`      | Clear the log.                                                  |
| `rchelp [COMMAND]` | Show the command index or help for one command.                 |

### Browse and inspect

| Command    | What it does                                      |
| :--------- | :------------------------------------------------ |
| `rcl`      | List immediate files and folders with formatting. |
| `rcd`      | List immediate folders.                           |
| `rcls`     | List names, sizes, and modification dates.        |
| `rct`      | Show a tree two levels deep.                      |
| `rctree`   | Show the full tree.                               |
| `rcinfo`   | Show file totals and remote storage quota.        |
| `rcbrowse` | Open rclone’s interactive storage explorer.       |
| `rcpick`   | Pick a remote with fzf, then browse it.           |

### Mounts

| Command           | What it does                                  |
| :---------------- | :-------------------------------------------- |
| `rcmount REMOTE`  | Mount at `~/<remote>-rclone`.                 |
| `rcumount REMOTE` | Unmount and remove the mount folder if empty. |

### Transfers

Paths always go **source first, destination second**.

| Command       | What it does                                     |
| :------------ | :----------------------------------------------- |
| `rccopy`      | Copy between local or remote paths.              |
| `rcto`        | Copy a file to an exact destination filename.    |
| `rcpull`      | Copy remote → local.                             |
| `rcpush`      | Copy local → remote.                             |
| `rcdry-down`  | Preview a remote → local copy.                   |
| `rcdry-up`    | Preview a local → remote copy.                   |
| `rcplan`      | Preview a sync, including destination deletions. |
| `rcmirror`    | Sync any source and destination.                 |
| `rcsync-down` | Sync remote → local.                             |
| `rcsync-up`   | Sync local → remote.                             |

Copies can overwrite files but keep extra destination files. **Sync can delete extra destination files.** Preview with `rcplan` or `--dry-run`.

### Comparisons

Pass **remote first, local path second**. These commands do not change files.

| Command         | What it shows                                           |
| :-------------- | :------------------------------------------------------ |
| `rcstatus`      | Combined comparison with a symbol legend.               |
| `rcremote-only` | Files missing locally.                                  |
| `rclocal-only`  | Files missing remotely.                                 |
| `rcdiff`        | Files present on both sides but different.              |
| `rcgit`         | Separate local-only, remote-only, and changed sections. |

## Examples

Replace `drive` with a configured remote from `rcremotes`.

```bash
rcl drive
rctree drive:Documents --level 3 --size
rcpull drive:Documents ./Documents
rcdry-up ./Documents drive:Documents
rcplan ./Documents drive:Documents
rcgit drive:Documents ./Documents
```

Helpers accept `drive` or `drive:` for a remote root and `drive:Folder` for a subfolder. Quote paths containing spaces and put options after the paths. Use `./drive` for a local folder named like a configured remote.

## Setup

Requires Zsh and rclone. Source `rcaliases.zsh` from your `.zshrc`:

```bash
source /path/to/rclone-alias/rcaliases.zsh
```

The script expects these resources under `$ZDOTDIR`:

- `docs/rclone/`: help files included in this repo.
- `tools/rclone/rclone-format.py`: listing formatter, currently external to this repo.

Formatted listings require Python 3. Additional commands use `fzf` (`rcpick`), `bat` (`rcconfig`), and `nvim` (`rcedit`). Mount helpers use `mountpoint` and `fusermount`. A Nerd Font displays listing icons.
