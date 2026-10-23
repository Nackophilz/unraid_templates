# unraid_templates
Centralization for unraid template

## Available templates

| | Name | Description | Upstream |
|---|---|---|---|
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/actual-budget.png" width="32" alt="Enable Actual"> | [Enable Actual](enable-actual/enable-actual.xml) | Imports European bank transactions into Actual Budget through Enable Banking | [2manyvcos/enable-actual](https://github.com/2manyvcos/enable-actual) |
| <img src="https://raw.githubusercontent.com/Masutayunikon/FanKarr/main/public/fankai.png" width="32" alt="FanKarr"> | [FanKarr](fankarr/fankarr.xml) | Download manager for the Fankai catalogue (Kai & Yabai editions), inspired by Radarr/Sonarr | [Masutayunikon/FanKarr](https://github.com/Masutayunikon/FanKarr) |
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/fluidd.png" width="32" alt="Fluidd"> | [Fluidd](fluidd/fluidd.xml) | Klipper web interface for 3D printers | [fluidd-core/fluidd](https://github.com/fluidd-core/fluidd) |
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/freebox-ultra.png" width="32" alt="Freebox OS Ultra Dashboard"> | [Freebox OS Ultra Dashboard](freeboxos-ultra-dashboard/freeboxos-ultra-dashboard.xml) | Web dashboard to manage a Freebox (Ultra, Delta, Pop) | [HGHugo/FreeboxOS-Ultra-Dashboard](https://github.com/HGHugo/FreeboxOS-Ultra-Dashboard) |
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/google-photos.png" width="32" alt="gPhotos2Immich"> | [gPhotos2Immich](gphotos2immich/gphotos2immich.xml) | Syncs Google Photos shared albums to Immich on a schedule | [warreth/gPhotos2Immich](https://github.com/warreth/gPhotos2Immich) |
| <img src="https://raw.githubusercontent.com/anthonyraymond/joal-desktop/master/resources/icons/1024x1024.png" width="32" alt="JOAL"> | [JOAL](joal/joal.xml) | Ratio master with a web UI | [anthonyraymond/joal](https://github.com/anthonyraymond/joal) |
| <img src="https://raw.githubusercontent.com/dinglebear-ai/unraid/main/plugins/mcp/source/usr/local/emhttp/plugins/unraid-mcp/unraid-mcp.png" width="32" alt="Unraid MCP"> | [Unraid MCP](unraid-mcp/unraid-mcp.xml) | MCP server to query and manage an Unraid server through its GraphQL API | [dinglebear-ai/unraid](https://github.com/dinglebear-ai/unraid) |

## Things to know

- **Enable Actual**: the container runs as `1001:1001`. Before the first start, run `mkdir -p /mnt/user/appdata/enable-actual && chown -R 1001:1001 /mnt/user/appdata/enable-actual`, otherwise it stops with a permission error. Set the image tag to your Actual Budget server version (e.g. `2manyvcos/enable-actual:actual26.10.0`): the Actual client in the image must be the same version as the server or newer. `PUBLIC_URL` must be the exact address you open the UI with, otherwise the UI cannot reach its API. There is no built-in authentication.
- **FanKarr**: completed downloads and the media library must both live under the single `Media` path, otherwise hardlinks fail and files are copied. If the torrent client runs in another container, set its remote path (as seen by the client) and local path (as seen by FanKarr) in FanKarr's client settings.
- **Fluidd**: add the Fluidd URL (`http://UNRAID-IP:PORT`) to `cors_domains` in the printer's `moonraker.conf`, and use the printer's real IP in camera stream URLs.
- **Freebox OS Ultra Dashboard**: the container runs as `1001:1001`. Before the first start, run `mkdir -p /mnt/user/appdata/freebox-dashboard && chown -R 1001:1001 /mnt/user/appdata/freebox-dashboard`, otherwise the Freebox token is not saved. On first connection, approve the app on the Freebox screen.
- **gPhotos2Immich**: `config.json` is mounted as a single file. Before the first start, run `mkdir -p /mnt/user/appdata/gphotos2immich && echo '{}' > /mnt/user/appdata/gphotos2immich/config.json`, otherwise Docker creates a folder in its place and settings cannot be saved. Only public shared album links work. The web UI has no password.
- **JOAL**: choose your own path prefix and secret token; the UI is at `http://UNRAID-IP:PORT/PATH_PREFIX/ui/`, so the template has no WebUI link (add it in the WebUI field, advanced view). The image needs internet access at every start to download `tzdata`.
- **Unraid MCP**: needs the Unraid API (built into Unraid 7.2+, Unraid Connect plugin on older releases) and an API key from Settings › Management Access › API Keys. MCP clients connect to `http://UNRAID-IP:6970/mcp` with the header `Authorization: Bearer <token>`. The server can stop the array, containers and VMs: keep its port off the internet. The image is now published as `ghcr.io/dinglebear-ai/unraid-mcp` (the old `ghcr.io/jmagar/unraid-mcp` stopped at 2.4.0) and is amd64 only.
