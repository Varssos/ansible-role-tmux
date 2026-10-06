# tmux

Ansible role to install [tmux](https://github.com/tmux/tmux), the Meslo Nerd Font, the tmux plugin manager ([tpm](https://github.com/tmux-plugins/tpm)), and stow personal tmux dotfiles config on Debian/Ubuntu systems.

## Requirements

- Debian or Ubuntu host
- `become: true` privileges (sudo)
- Depends on the `dotfiles` role (clones `~/dotfiles`, which must contain a `tmux` stow package; provides `dotfiles_path` and `user_home_path`). `ansible_user` must be set

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `tmux_packages` | `[tmux, xclip]` | Packages to install via apt |
| `tmux_meslo_fonts_dir` | `/usr/share/fonts/opentype/Meslo` | Install directory for the Meslo Nerd Font |
| `tmux_nerd_font_version` | `v3.4.0` | Nerd Fonts release tag to download |
| `tmux_nerd_font_name` | `Meslo` | Nerd Fonts archive name to download |
| `tmux_tpm_repo` | `https://github.com/tmux-plugins/tpm.git` | Git repo for the tmux plugin manager |

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: dotfiles
    - role: tmux
```

Override variables if needed:

```yaml
- hosts: all
  become: true
  roles:
    - role: dotfiles
    - role: tmux
      vars:
        tmux_nerd_font_version: "v3.5.0"
```

## Manual setup (without Ansible)

```
sudo apt update && sudo apt install tmux git xclip # or xsel for tmux-yank
git clone https://github.com/tmux-plugins/tpm.git ~/.tmux/plugins/tpm

mkdir -p ~/dotfiles
cd ~/dotfiles
stow tmux

~/.tmux/plugins/tpm/scripts/source_plugins.sh
~/.tmux/plugins/tpm/scripts/install_plugins.sh
```

In case of any modifications to the tmux config, source the changes:
```
tmux source ~/.config/tmux/tmux.conf
```

## Known issues

- The tpm `source_plugins.sh`/`install_plugins.sh` scripts sometimes return a non-zero exit code even when everything is fine — handled with `ignore_errors`/`|| true` in the tasks.

## License

MIT

## Author

Varssos