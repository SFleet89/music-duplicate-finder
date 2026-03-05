# Changelog — undo_duplicates.py

All notable changes to the Music Duplicate Undo script are documented here.

---

## [2.1] - 2026-03-04

### Added
- **`--list-categories` flag** — prints all unique category values found in the CSV report along with row counts and how many were moved. Useful for checking valid values before using `--filter-category`.
- **Compatibility note in docstring** — clarifies that `Match Method` and `Why Not Exact` columns present in v3.1+ reports are intentionally ignored.

### Fixed
- **`--filter-category` docstring** — previously listed `"Standard Duplicate"` and `"Higher Quality Match"` as the valid category values. Corrected to reflect the actual CSV values used by the main script: `"Exact Match"`, `"DUPLICATE"`, and `"BETTER QUALITY"`. Old v3.0 labels are still accepted for backward compatibility.

### Changed
- Version bump to v2.1 to reflect compatibility with find_music_duplicates.py v3.1 and v3.2 CSV formats.
- Usage/help output updated to show all available options and include `--list-categories` example.

---

## [2.0] - 2026-02-27

### Added
- Full rewrite to support the v3.0 CSV report format from find_music_duplicates.py.
- `--dry-run` flag — preview restores without moving anything.
- `--filter-category` flag — undo only a specific category of files.
- Handles missing files and conflicts gracefully (skips with explanation rather than crashing).
- Summary at end of run showing restored, skipped, and error counts.
- Skips dry-run rows from the CSV automatically (rows with "Would move" actions).

---

## [1.0] - 2026-02-27

### Added
- Initial script — reads log file from find_music_duplicates.py and moves files back to original locations.
- Basic error handling.
