# Terminus Mass Update Plugin (Terminus 3/4 Compatible)

A Terminus plugin to apply upstream updates to multiple sites at once, with post-deploy tasks.

## Features

- **Terminus 3 & 4 compatible** — handles the `checkProgress` → `WorkflowProcessingTrait` migration
- **Organization support** — use `--org=<uuid>` instead of piping from `site:list`
- **Post-deploy tasks** — run `drush updb`, `drush cim`, and `drush cr` after applying updates
- **Frozen site detection** — automatically skips frozen sites
- **Git mode check** — skips sites not in git mode
- **Dry run mode** — preview what would be updated without making changes
- **Summary report** — shows total/succeeded/failed/skipped counts

## Installation

```bash
terminus self:plugin:install /path/to/terminus-mass-update-v2
```

Or from a git repository:

```bash
terminus self:plugin:install https://github.com/YOUR_ORG/terminus-mass-update.git
```

## Usage

### Apply Updates

Using organization UUID (recommended for your setup):

```bash
# Apply upstream updates to all sites in your org
terminus site:mass-update:apply --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008 --accept-upstream

# Apply with post-deploy tasks
terminus site:mass-update:apply \
  --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008 \
  --accept-upstream \
  --updatedb \
  --config-import \
  --cache-clear

# Dry run first
terminus site:mass-update:apply \
  --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008 \
  --accept-upstream \
  --dry-run

# Filter by specific upstream
terminus site:mass-update:apply \
  --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008 \
  --upstream=YOUR_UPSTREAM_UUID \
  --accept-upstream \
  --updatedb \
  --config-import \
  --cache-clear
```

Using STDIN (original behavior):

```bash
terminus site:list --format=list | terminus site:mass-update:apply --accept-upstream --updatedb
terminus org:site:list --format=list | terminus site:mass-update:apply --accept-upstream
```

### List Available Updates

```bash
# List updates for all sites in org
terminus site:mass-update:list --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008

# List with STDIN
terminus site:list --format=list | terminus site:mass-update:list
```

### Options

| Option | Description |
|---|---|
| `--org=<uuid>` | Fetch sites from a specific organization |
| `--upstream=<uuid>` | Filter to only sites using this upstream |
| `--accept-upstream` | Auto-resolve merge conflicts in favor of upstream |
| `--updatedb` | Run `drush updb -y` after applying updates |
| `--config-import` | Run `drush cim -y` after applying updates |
| `--cache-clear` | Run `drush cr` after applying updates |
| `--dry-run` | Preview updates without applying them |
| `--skip-frozen` | Skip frozen sites (default: true) |

## Changes from Original Plugin

1. **Terminus 4 compatibility** — replaced `$workflow->checkProgress()` with compatible workflow processing
2. **`--org` option** — fetch sites directly from an organization without piping
3. **`--config-import` option** — run config import after updates
4. **`--cache-clear` option** — clear caches after updates
5. **Error resilience** — one site failing doesn't stop the rest
6. **Summary report** — shows counts at the end
7. **Frozen site detection** — auto-skips frozen sites
8. **PHP 8.2+ required** — matches Terminus 4 requirements
