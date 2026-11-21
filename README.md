# Campus Cleanliness Reporter

StudentLife Hack - Project #42 | Environment & Sustainability

A menu-driven Python app to **record cleanliness reports by location** and **surface recurring problem areas**.
Standard library only; `matplotlib` is optional and used just for charts.

## Quick start

```bash
cd campus_cleanliness_reporter
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt   # only needed for charts
python main.py
```
Requires Python 3.10+. On first launch the app offers to load 41 sample reports.

Run the tests: `python -m unittest discover -s tests -v`

## Features

| Hackathon requirement | Where |
|---|---|
| Interactive terminal menu | `ccr/cli.py`, `ccr/ui.py` |
| Defensive input validation (dates, 1-5 rating, lengths, allowed characters, choices) | `ccr/validation.py` + retry loops in `ui.ask()` |
| Core logic: reports by location, recurring areas | `ccr/analytics.py` (`location_stats`, `recurring_areas`) |
| JSON persistence, atomic writes, corrupt-file recovery | `ccr/storage.py` |
| Keyword / category / rating search | menu 3, `analytics.filter_reports` |
| Executive summary (totals, averages, status, trend) | menu 4, `analytics.summarize` |
| Matplotlib charts (4 PNGs) | menu 6, `ccr/plotting.py` |
| Export Markdown / TXT audit log / CSV | menu 7, `ccr/export.py` |
| Threshold warnings (hotspot, recurring, critical) | `analytics.warnings` |
| Bulk CSV import with row-by-row error log | menu 8, `ccr/importer.py` |

## Business rules

- **Rating:** 1 (filthy) to 5 (spotless). Status: 1-2 = Critical, 3 = Needs Attention, 4-5 = Good.
- **Recurring area:** a location with **2+ reports rated <= 2**.
- **Hotspot warning:** 3+ low reports at one location within 14 days.
- Thresholds are constants at the top of `ccr/analytics.py` - easy to tune.

## Project layout

```
main.py                      entry point
ccr/                         application package
data/sample_reports.csv|json 41 realistic sample records (incl. boundary cases)
data/legacy_import_with_errors.csv   messy file to demo the importer's error log
generate_sample_data.py      regenerates the sample files
tests/test_core.py           20 unit tests
exports/                     charts, reports and logs are written here
```

## 3-minute demo script

1. Launch `python main.py` -> branded screen -> load sample data -> see warnings.
2. **1 Record a report**: type a bad date, rating `9`, rating `abc`, and watch each get rejected, then enter valid values.
3. **4 Summary** and **5 Recurring areas** -> Hostel restroom & cafeteria flagged.
4. **6 Charts** and **7 Export** -> open files in `exports/`.
5. **8 Import** the default `legacy_import_with_errors.csv` -> see skipped rows + log file.
6. Exit, relaunch, show the data is still there (persistence).

## Ideas for next steps

Tkinter GUI front-end, per-location trend charts, email/Slack alerts on hotspots, photo attachments.
