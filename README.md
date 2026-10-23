# XPAIS-Traffic ShipDB Community Merger

A static browser tool for comparing a user's ShipDB against the central `ship_db.csv` in this repository.

## Repository files
- `index.html`: the browser app.
- `ship_db.csv`: central reference database loaded by the app.

## User workflow
1. Open the GitHub Pages site.
2. The app loads the repository's central `ship_db.csv`.
3. Select your local ShipDB and compare.
4. Download `ship_db_merged.csv` for your own use and/or `ship_db_contribution.csv` to propose new records and missing values.
5. Submit contributions through a fork and Pull Request. Review all changes before merging.

The website is static and cannot write to the repository. CSVs are processed in the browser and are not uploaded by the app. The central database changes only when a reviewed Pull Request is merged.
