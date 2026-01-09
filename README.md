# DDEV Claude Code Add-on

This DDEV add-on installs [Claude Code](https://claude.ai/code) for the current user inside the DDEV web container.

## Installation

```bash
ddev add-on get /path/to/ddev-claude
ddev restart
```

## Usage

```bash
ddev claude
ddev claude --help
ddev claude doctor
```

Or via SSH:

```bash
ddev ssh
claude
```

## How it works

- Installs Claude Code on first `ddev restart`
- **Config/auth** stored in `.ddev/claude/` (host-accessible, persists across restarts)
- **Binaries** stored in a Docker volume (isolated from host)
- Symlinks `~/.claude`, `~/.claude.json`, `~/.local/bin`, `~/.local/share` appropriately

## Configuration

Claude Code settings are stored in `.ddev/claude/`:

- `.ddev/claude/.claude.json` - settings and auth
- `.ddev/claude/.claude/` - CLAUDE.md, skills, etc.

The `.gitignore` excludes auth tokens from version control.

### Adding CLAUDE.md or skills (per project)

Place files directly in `.ddev/claude/.claude/`:

```bash
echo "My project instructions" > .ddev/claude/.claude/CLAUDE.md
mkdir -p .ddev/claude/.claude/skills
```

### Global CLAUDE.md and skills (all projects)

Use DDEV's homeadditions to share across all projects. Create:

```
~/.ddev/homeadditions/.claude-global/CLAUDE.md
~/.ddev/homeadditions/.claude-global/skills/
```

These will be copied (without overwriting) to each project's config on `ddev restart`.

## Removal

```bash
ddev add-on remove claude
ddev restart
```

To also remove the binaries volume:

```bash
docker volume rm ddev-<projectname>_claude-binaries
```

## License

MIT
