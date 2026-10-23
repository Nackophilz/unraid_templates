# unraid_templates
Centralization for unraid template

## Available templates

| | Name | Description | Upstream |
|---|---|---|---|
| <img src="https://raw.githubusercontent.com/Masutayunikon/FanKarr/main/public/fankai.png" width="32" alt="FanKarr"> | [FanKarr](fankarr/fankarr.xml) | Download manager for the Fankai catalogue (Kai & Yabai editions), inspired by Radarr/Sonarr | [Masutayunikon/FanKarr](https://github.com/Masutayunikon/FanKarr) |
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/fluidd.png" width="32" alt="Fluidd"> | [Fluidd](fluidd/fluidd.xml) | Klipper web interface for 3D printers | [fluidd-core/fluidd](https://github.com/fluidd-core/fluidd) |
| <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/png/freebox-ultra.png" width="32" alt="Freebox OS Ultra Dashboard"> | [Freebox OS Ultra Dashboard](freeboxos-ultra-dashboard/freeboxos-ultra-dashboard.xml) | Web dashboard to manage a Freebox (Ultra, Delta, Pop) | [HGHugo/FreeboxOS-Ultra-Dashboard](https://github.com/HGHugo/FreeboxOS-Ultra-Dashboard) |
| <img src="https://raw.githubusercontent.com/anthonyraymond/joal-desktop/master/resources/icons/1024x1024.png" width="32" alt="JOAL"> | [JOAL](joal/joal.xml) | Ratio master with a web UI | [anthonyraymond/joal](https://github.com/anthonyraymond/joal) |

## Things to know

- **FanKarr**: completed downloads and the media library must both live under the single `Media` path, otherwise hardlinks fail and files are copied. If the torrent client runs in another container, set its remote path (as seen by the client) and local path (as seen by FanKarr) in FanKarr's client settings.
- **Fluidd**: add the Fluidd URL (`http://UNRAID-IP:PORT`) to `cors_domains` in the printer's `moonraker.conf`, and use the printer's real IP in camera stream URLs.
- **Freebox OS Ultra Dashboard**: the container runs as `1001:1001`. Before the first start, run `mkdir -p /mnt/user/appdata/freebox-dashboard && chown -R 1001:1001 /mnt/user/appdata/freebox-dashboard`, otherwise the Freebox token is not saved. On first connection, approve the app on the Freebox screen.
- **JOAL**: choose your own path prefix and secret token; the UI is at `http://UNRAID-IP:PORT/PATH_PREFIX/ui/`, so the template has no WebUI link (add it in the WebUI field, advanced view). The image needs internet access at every start to download `tzdata`.
