# Walkthrough — digital-forensics-toolkit

How each area works, plus the cleanup done to the repo.

## metadata_scanners/

Files carry hidden metadata that's gold for an investigator — when a photo was
taken, which camera, sometimes GPS coordinates; who authored a document and when.

- **metadata_pictures.py** — reads EXIF tags from an image with Pillow (`PIL.ExifTags`) and can export GPS coordinates to a KML file (`simplekml`) to plot on a map.
- **metadata_pdf.py** — pulls document info (author, creator, dates) from a PDF with PyPDF2.
- **metadata_msoffice.py** — reads core properties from Office documents.
- **metadata_video_audio.py** — reads tags from media files with `mutagen`.
- **metadata_executable.py** — parses PE (Windows executable) headers with `pefile` — useful for malware triage.

## hashing/

Hashes are how forensics proves a file hasn't changed — the same input always
produces the same hash, so it's a fingerprint for evidence integrity.

- **file_hashing.py** — hashes a file's name and contents with a chosen algorithm (md5/sha1/sha256/sha512). Verified: hashing the sample files produces stable, correct digests.
- **dir_tree_hash.py** — walks a directory and hashes every file, so you can snapshot and later verify an entire tree.
- **disk_hash.py** — hashes a large source (like a disk image) in chunks.

## threat_intel/

Turning indicators (domains, file hashes) into a verdict.

- **virustotal/** — `domain_query.py` and `file_query.py` submit domains/IPs and file hashes to the VirusTotal API and write the results to CSV. The API key is read from `key.txt` (see `key.txt.example`), never hardcoded.
- **passive_dns/** — looks up domain information from passive DNS sources.
- **beautifulsoup_parser.py** — scrapes and parses web pages with BeautifulSoup.

## forensic_recipes/

Smaller building-block scripts:

- **os_dir_walk.py** — recursively walk a filesystem (the basis for any file-collection tool).
- **multi_process.py** — speed up heavy jobs with the `multiprocessing` module.
- **arguments.py** — a clean `argparse` CLI template.
- **metadata_files.py** — collect filesystem timestamps (created/modified/accessed).
- **logging_example.py** — proper logging, important for a defensible audit trail.

## Repository cleanup

- **Removed a 52 MB `Firefox Setup 95.0.exe`** that had been committed by accident — it made the repo 57 MB; it's now 400 KB.
- **Removed a duplicated third-party Bootstrap dashboard template** (hundreds of vendor CSS/JS/font files that weren't original code) and an unrelated, incomplete Dash project.
- Moved two Python-basics notebooks (comprehensions) to the `python-fundamentals` repo, where they belong.
- Normalized every mangled shebang (`#!/ur/bin/env`, `#!bin?usr/env`, …) to `#!/usr/bin/env python3`.
- Renamed `key.txt` to `key.txt.example` and gitignored the real key file.
- Removed a `DOESN'T-WORK-YET-...` script and generated log/report files.
