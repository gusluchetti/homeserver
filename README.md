# home server

Docker Compose stacks for my home server: a media "lab", a modded Minecraft server and an
[ArchiSteamFarm](https://github.com/JustArchiNET/ArchiSteamFarm) instance.

## layout

| path | what's in it |
| --- | --- |
| `groups/docker-compose.lab.yaml` | media stack: qBittorrent, Jellyfin, Prowlarr, Sonarr, Radarr, Bazarr |
| `groups/docker-compose.gaming.yaml` | Minecraft server running the All the Mods 10 modpack via CurseForge |
| `asf/` | ArchiSteamFarm (card idling) |

## running

Each stack is started on its own:

```bash
docker compose -f groups/docker-compose.lab.yaml up -d
docker compose -f groups/docker-compose.gaming.yaml up -d
docker compose -f asf/docker-compose.yaml up -d
```

The lab stack expects data under `/home/lab/appdata` and `/home/lab/data`, and runs as UID/GID 1000.
Change the volume paths, `PUID`/`PGID` and `TZ` to match your machine.

| service | port |
| --- | --- |
| qBittorrent web UI | 8081 |
| Jellyfin | 8096 |
| Prowlarr | 9696 |
| Sonarr | 8989 |
| Radarr | 7878 |
| Bazarr | 6767 |
| Minecraft | 25565 |

## secrets

Secrets come from a `.env` file next to the compose file (gitignored):

| variable | used by |
| --- | --- |
| `CF_API_KEY` | Minecraft: CurseForge API key for downloading the modpack |
| `ASF_STEAM_PASSWD` | ASF: Steam password, read through `PasswordFormat: 3` (environment variable) |

The Minecraft `OPS` and `WHITELIST` lists hold player UUIDs; replace them with your own.

### ASF config

The real ASF configs are gitignored because they contain your Steam login and SteamID. Copy the examples:

```bash
cp -r asf/config.example asf/config
```

Then set `SteamOwnerID` in `ASF.json`, and rename `mybot.json` to your bot's name with your Steam
login inside it.

## todos
- [ ] NAS-like control for files
- [ ] game streaming from office to living room (wired steam remote play should work?)
