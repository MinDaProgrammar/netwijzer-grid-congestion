# NetWijzer — Grid-Congestion Planning Map for Drenthe (PowerUp Drenthe Hackathon)

**Live demo:** https://mindaprogrammar.github.io/netwijzer-grid-congestion/app/ (Dutch by default, EN toggle in the top bar)

**Pitch video (2 min):**

[![NetWijzer pitch video](https://img.youtube.com/vi/L8tetGbi6bU/hqdefault.jpg)](https://www.youtube.com/watch?v=L8tetGbi6bU)

The Dutch electricity grid is full in many regions: companies and energy projects wait years for a connection without knowing when or why, while grid planners decide on upgrades without seeing which projects depend on them. NetWijzer is a web app that puts everyone's energy plans in **one shared register** with two views of the same data:

- **Customers** (project developers, companies, municipalities) see when they can expect a connection, why, what they are waiting on, and what could get them connected sooner
- **Planners** (grid operators, the province) see which areas run out of grid capacity in which year, which projects wait on each upgrade, and what has to be decided by when

Built by Team 12 in 30 hours at the **PowerUp Drenthe** grid-congestion hackathon (online, 12–13 November 2026), organised by the Province of Drenthe with TenneT, Enexis, RENDO and Netbeheer Nederland. Our track was set by TenneT: *"Where do you tackle grid congestion most smartly? Build the regional measures map."*

## Features

**Customer side**
- *My projects:* every connection request with its expected connection date (or "under review"), status, next step and who owns it
- *Project page:* the reason behind the date ("waiting on …"), the real capacity-map status and queue for that location, cable distance to the substation, ways to connect sooner (battery, flexible contract, cable pooling), and a timeline of every change and why
- *New request:* pick a location on the map and see the grid status there; the request goes to a grid-operator expert for review instead of returning an instant (and misleading) estimate

**Planner side**
- *Overview map:* today's real capacity map, or a projection for any year up to 2035, with layers for substations, planned grid upgrades, TenneT's high-voltage grid and projects
- *Area page:* connected load vs capacity per year, demand that doesn't fit (formal requests vs early plans), and the decision needed: biggest shortfall, projects waiting on it, owner and deadline
- *Simulator:* delay or cancel an upgrade, add a new one, place a battery or withdraw a request, and see every project's connection date before and after; confirming the decision writes the change into each affected customer's timeline

## Data

- **Real:** capacity-map status and queues per supply area (Netbeheer Nederland), TenneT high-voltage lines and stations, grid upgrades with desired and feasible years (Enexis investment plan 2026), CBS / PDOK boundaries and basemap, OpenStreetMap substations
- **Demo, labelled as such in the app:** all projects and customers, MW per station area (2026 loads calibrated to the real capacity-map pattern by `scripts/calibrate_areas.py`), upgrade sizes and battery effects

Sources are listed in [`data/README.md`](data/README.md) and [`docs/RESEARCH.md`](docs/RESEARCH.md).

## How it works

A static single-page app: no build step, no backend.

```
app/
├── index.html      # entry point
├── js/store.js     # loads the GeoJSON / JSON data, holds the shared register and app state
├── js/engine.js    # per area and year: load vs capacity, unmet demand, expected connection dates
├── js/pages.js     # customer and planner pages, simulator
├── js/mapkit.js    # Leaflet maps (PDOK basemap), layers and popups
└── js/i18n.js      # Dutch / English strings
data/app/           # demo register: plans, areas, upgrades
data/real/          # processed open data
scripts/            # Python pipeline: fetch open data, build and calibrate the demo data
docs/               # plan, research notes with sources, data schema for the shared intake format
```

Run it locally:

```bash
python3 -m http.server 8000
# open http://localhost:8000/app/
```

The raw capacity-map downloads (about 230 MB) aren't in the repo; `scripts/fetch_capaciteitskaart.py` and `scripts/prepare_real_data.py` rebuild `data/real/` from the public sources.

## Team and my role

Team 12: Minseok Choi, Vaishanavi Mehta and Yuri Makarevich.
<!-- TODO(Minseok): add anyone else on the team (e.g. who made the pitch video) -->

Original team repository: [vaishsRP/Powerup-DrentheTeam12](https://github.com/vaishsRP/Powerup-DrentheTeam12).

- **Me:** built the first clickable map prototype (PDOK basemap, grid layers, zoom detail, the grid-infrastructure fetch script), then developed the NetWijzer app in `app/` with Claude Code: the customer and planner views on real capacity-map and TenneT data, the simulator, battery placement, unmet-demand figures and the infrastructure details asked for by the challenge experts. I merged each feature into the team repository as it was finished.
- **Vaishanavi:** research and official sources, the TenneT investment-plan bottlenecks, the shared data schema and the pre-pitch
- **Yuri:** the planning-page concept, the customer journey and map layers

This repository is a fork of the team's repository, so the full development history is kept here.
