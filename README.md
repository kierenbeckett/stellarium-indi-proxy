# Stellarium INDI Proxy

Stellarium INDI Proxy lets you connect to any INDI compatible telescope mount from inside the Stellarium Mobile app.

## Quick Start

* Ensure you have INDI server running and connected to a mount
* Ensure you have python3 installed
* Download and run the [stellarium_indi_proxy](stellarium_indi_proxy) script with the name of your mount driver in INDI e.g.

```
./stellarium_indi_proxy "EQMod Mount"
```

* Open up [Stellarium Mobile](https://www.stellarium-labs.com/)
* Ensure you have the Pro version
* Ensure `Observing Tools > Telescope Control` is enabled
* On the telescope control bar at the bottom of the screen, select Type as Network
* Enter the Host/Port of the proxy (default 7625) and click to connect
* You should get green ticks next to Aligned, Time and Location and be able to see/control your scope

## Usage

```
usage: stellarium_indi_proxy [-h] [--reverse-ra] [--reverse-dec] [--host HOST] [--port PORT] [--indi-host INDI_HOST] [--indi-port INDI_PORT] mount

Stellarium INDI Proxy

positional arguments:
  mount                 The name of the mount to be controlled in INDI

options:
  -h, --help            show this help message and exit
  --reverse-ra          Reverse RA buttons so that pressing left increases the RA
  --reverse-dec         Reverse DEC buttons so that pressing down increases the DEC (or decreases when pier side east, pointing west)
  --host HOST           Host to bind to (default: 127.0.0.1)
  --port PORT           Port to listen on (default: 7625)
  --indi-host INDI_HOST
                        INDI host (default: 127.0.0.1)
  --indi-port INDI_PORT
                        INDI port (default: 7624)
```

## Run in systemd

Create a systemd service at `/etc/systemd/system/stellarium-indi-proxy.service`

```
[Unit]
Description=Stellarium INDI Proxy
After=multi-user.target

[Service]
Type=idle
User=youruser
ExecStart=/path/to/stellarium_indi_proxy "EQMod Mount"
Restart=always
RestartSec=60

[Install]
WantedBy=multi-user.target
```

Enable and start the service

```
systemctl enable stellarium-indi-proxy.service
systemctl start stellarium-indi-proxy.service
```

## FAQs

### Why is this needed, why not connect Stellarium directly to the mount?

INDI has support for a wider range of mounts than is [supported directly](https://stellarium-labs.com/telescope-control-in-stellarium-mobile-plus/).

Even if your mount is supported directly it can still be beneficial. For example with a Synscan based mount you can use this script with INDI and EQMod to connect directly to the mount control box without the handset. You can then connect concurrently from Stellarium and other INDI clients such as Kstars/Ekos.

### What mounts are supported?

In theory any mount supported by INDI should work. So far it has only been tested on the EQMod driver. If it works for your mount please let me know.

### How do I run INDI server?

There are many ways to run INDI server

* Have [Kstars/Ekos](https://kstars.kde.org/) run it for you
* Setup [Astroberry](https://www.astroberry.io/)
* Use [INDI Web Manager](https://github.com/knro/indiwebmanager)
* Run it directly via systemd

I personally use a Raspberry Pi 2 Zero with Ubuntu installed to run INDI server and this script via systemd. I can then connect to it from Stellarium on my phone or Ekos from my laptop.

## License

Distributed under the GNU General Public License v3.0. See `LICENSE` for more information.

## Author

Written by [Kieren Beckett](http://kierenb.net).
