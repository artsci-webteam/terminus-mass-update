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
git clone https://github.com/artsci-webteam/terminus-mass-update.git
terminus self:plugin:install /path/to/terminus-mass-update
```

## Usage

### Apply Updates
```bash
# Apply upstream updates to a single site
echo "wustl-itartsci" | terminus site:mass-update:apply --accept-upstream
```

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
terminus site:list --format=list | terminus site:mass-update:apply --accept-upstream
```

### List Available Updates

```bash
# List updates for all sites in org
terminus site:mass-update:list --org=885ff9a3-c5b0-4d79-baea-3eb9f4dd0008

# List with STDIN
terminus site:list --format=list | terminus site:mass-update:list
```

