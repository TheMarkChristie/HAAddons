# Shortlist add-on

Shortlist (by Steven Adams, MIT, https://github.com/stevezau/shortlist) builds per-person recommendation rows on Plex
Home from each person's watch history: Because you watched, Picked for You, Watch it again, and more. This add-on is
a thin wrapper around the published image; nothing is built or changed here.

## Requirements

- Plex Media Server 1.43.2.10687 or newer, with Plex Pass on the server owner's account.
- A free TMDB API key (themoviedb.org, no card).
- This Home Assistant host can reach the Plex server and plex.tv.

## Install

1. Settings > Add-ons > Add-on store, the three-dots menu, **Repositories**: add
   `https://github.com/TheMarkChristie/HAAddons`. Shortlist appears in the store (reload the page if not).
2. Install, then Start. Open Web UI opens `http://<home-assistant>:5959`.
3. The setup wizard: sign in with Plex (PIN), pick the server, enter the TMDB key, choose **None** for the AI provider,
   tick the people who get rows, name the first row, and let the first run go.

The first sign-in claims this Shortlist instance: do it from the home network.

## Dry run first

`SHORTLIST_DRY_RUN` is `"1"` in this add-on's `config.yaml` to begin with, so every run (the wizard's first run
included) logs what it would write to Plex and writes nothing. Read Runs > How we picked. When the picks look right,
the add-on repository publishes the same add-on with dry run off as its next version: **Check for updates** on the
add-on page and Update. (Or, in Shortlist itself, start a run from the Runs page once the switch is off.)

## Updating Shortlist

The repository's `version:` follows Shortlist's tags on ghcr.io/stevezau/shortlist (`1.12.1`, `1.12.0`, ...).
When it moves, the add-on page shows Update. Back up `/addon_configs/<id>_shortlist` first; the releases say so.

## Where things are

- Shortlist's data: `/addon_configs/<repository id>_shortlist` on the host (`/config` inside the container): `shortlist.db`,
  `secret.key`, `session.secret`, `client.id`, `backups/`, `logs/`.
- Web interface: port 5959 on the Home Assistant host. Inside Home Assistant, other add-ons and integrations reach it
  at `http://<repository id>-shortlist:5959` (the add-on page shows its hostname).
- Uninstalling Shortlist properly: in its web interface, Settings > System > Danger zone > Uninstall restores the
  Plex sharing filters and removes its collections. Removing the add-on alone leaves those in Plex.

## Environment

| Variable | Value here | Why |
| --- | --- | --- |
| `TZ` | `Europe/Oslo` | Schedules in local time |
| `PUID` / `PGID` | `0` | The add-on's config folder is root-owned |
| `SHORTLIST_DRY_RUN` | `"1"` until the first real run | Safety |
| `PORT` | image default `5959` | |
| `SHORTLIST_CONFIG` | image default `/config` | |
