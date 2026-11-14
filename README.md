# XPAIS-Traffic ShipDB Community Merger

A browser tool for XPAIS-Traffic Vessel Pack owners to compare their local ShipDB with the central community `ship_db.csv`, contribute new or missing vessel data for review, and keep `last_seen_utc` dates up to date.

This repository contains only the web pages, hosted on GitHub Pages. The central database is **not** stored in this repository. It is kept in private Cloudflare R2 storage and served by an authenticated backend (Cloudflare Worker). Access requires a personal access token.

## Repository files

* `index.html`: the user app (load, compare, download, contribute, last-seen sync).
* `admin.html`: the maintainer page for review and publication. Every admin function is checked on the server and requires the admin token.

## Access

Access is restricted to XPAIS-Traffic VesselPack owners. Request a personal access token by private message on X-Plane.org or through the Ko-fi Shop.

* The page does not save your token. Enter it each time you open the page.
* Tokens cannot be retrieved later. Store yours securely and do not share it.

## User workflow

1. Open the GitHub Pages site and enter your access token.
2. Load the central database.
3. Select your local `ship_db.csv` (from the plugin's `ShipDB` folder) and compare. Your file is read in the browser and is not uploaded.
4. Review the results: new MMSIs, missing central values you can fill, conflicting values, last-seen dates to synchronize, and skipped invalid records with the reason.
5. Optionally download:
   * a merged personal `ship_db.csv` for your own use;
   * the contribution CSV, to inspect it before submitting.
6. **Synchronize last-seen dates:** for vessels whose `ship_type`, `length_m` and `beam_m` match the central record exactly, a newer `last_seen_utc` date (UTC, `YYYY-MM-DD`, not in the future) is applied to the central database immediately. Only MMSI, the three core values and the date are sent. Limit: 10,000 updates per user per UTC day.
7. **Upload contribution for review:** sends only the contribution CSV (`mmsi,ship_type,length_m,beam_m`) to the private review queue. It does not change the central database.

## Contribution rules

* Exactly four columns: `mmsi,ship_type,length_m,beam_m`. `last_seen_utc` is not part of a contribution.
* MMSI with exactly nine digits, AIS ship type 1–99, length and beam greater than 0. A `ship_type` of 0 or an empty value counts as missing.
* Up to 10,000 records (2 MB) per contribution.
* Submitting the same contribution again does not create a duplicate.
* Implausible dimensions are not contributed: a length of 500 m or more (511/512 are AIS maximum values) or a beam greater than the length.
* Conflicting values are only included if you enable the conflict option. They are listed for the maintainer as hints and never replace existing central data.

## Review and publication

Contributions go through **Pending → In Review → Accepted → Published** (or **Rejected**).

Publication runs on the server against the current central database:

* Existing valid central values are never changed. Differing proposals are only listed for the maintainer.
* New MMSIs are added and missing fields are filled automatically when all selected contributions propose the same row and its dimensions are plausible.
* If contributions disagree, or a row is unusual (beam over 70 m, length under 3 m, beam over 70 % of the length), the vessel goes to a review list. It is only published if the maintainer picks one complete row; otherwise it is proposed again by later contributions.
* Rows with impossible dimensions are dropped.
* A new MMSI gets the UTC day of its submission as its first `last_seen_utc`.
* A backup of the previous central file is kept for every publication.
* Contributions are marked **Published** only after the new central file has actually been written.

## Privacy

* Your local database stays in your browser. Only the contribution CSV (on upload) and the matching vessels' MMSI, core values and date (on last-seen sync) are sent.
* The central database is not publicly downloadable.
* GitHub Pages only hosts the pages; all data access is controlled by the backend.
