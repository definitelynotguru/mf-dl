# mf-dl - MediaFire Bulk Downloader

A command-line tool for bulk downloading files from MediaFire. Supports individual files, folders, and crawling the web for MediaFire links.

## Background

This project was originally created by **Pyxia** and published at [gitgud.io/Pyxia/mf-dl](https://gitgud.io/Pyxia/mf-dl). It was featured in a tutorial by **themadprogramer** on [Data Horde](https://datahorde.org/how-to-archive-or-scrape-mediafire-files-using-mf-dl/).

If you have MediaFire links you want to archive at scale, consider submitting them to the [Archive Team](https://archiveteam.org/index.php?title=MediaFire).

## Features

- Bulk download files from MediaFire links (files and folders)
- Web crawler to discover MediaFire links on any website
- Archive mode with metadata preservation
- Multi-threaded downloading
- Supports various MediaFire URL formats

## Installation

### Requirements

- Python 3.6+
- pip

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/definitelynotguru/mf-dl.git
   cd mf-dl
   ```

2. Install dependencies:
   ```bash
   python3 -m pip install -r requirements.txt
   ```

## Usage

### Downloading Files (`mfdl.py`)

Create a file containing your MediaFire links (one per line, or comma/space separated):

```
https://www.mediafire.com/file/xxxxx/example.txt/file
https://www.mediafire.com/folder/xxxxx/FolderName
```

Download them:

```bash
python3 mfdl.py output_directory links.txt
```

#### Options

| Flag | Description | Default |
|------|-------------|---------|
| `--threads N` | Number of concurrent download threads | 6 |
| `--archive-mode` / `-a` | Flat directory layout with `.info.json` metadata | Off |
| `--only-meta` | Download only metadata files | Off |

#### Examples

```bash
# Standard download
python3 mfdl.py downloads links.txt

# Single thread (if MediaFire shows captchas)
python3 mfdl.py --threads 1 downloads links.txt

# Archive mode (preserves all metadata)
python3 mfdl.py --archive-mode archive links.txt

# Multiple input files
python3 mfdl.py downloads links1.txt links2.txt links3.txt
```

### Crawling for Links (`web_crawler.py`)

Discover MediaFire links on websites:

```bash
python3 web_crawler.py https://example.com/ links_found.txt
```

The crawler runs indefinitely until interrupted. Found links are appended to the output file.

#### Options

| Flag | Description | Default |
|------|-------------|---------|
| `--threads N` | Number of concurrent crawler threads | 6 |
| `--filter URL` | Only crawl pages containing this URL | None |
| `--regex PATTERN` | Filter using regex pattern | None |

#### Examples

```bash
# Crawl a specific site only
python3 web_crawler.py --filter https://example.com https://example.com/ results.txt

# Using regex
python3 web_crawler.py --regex "mediafire.*\.zip" https://example.com/ results.txt

# Combine with download
python3 mfdl.py downloads results.txt
```

### Cron Automation

You can automate downloads using cron to periodically check for new links:

```bash
# Download new links every hour
0 * * * * cd /path/to/mf-dl && python3 mfdl.py downloads links_found.txt
```

## Supported Link Formats

- `https://*.mediafire.com/?KEY`
- `https://*.mediafire.com/*/KEY`
- `https://*.mediafire.com/*.php?KEY`
- `https://*.mediafire.com/convkey/*/KEY??.EXT`
- `https://*.mediafire.com/CUSTOM_FOLDER_NAME`
- `https://*.mediafire.com/?sharekey=SHAREKEY`
- `mfi.re` short links

## Archive Mode Directory Structure

When using `--archive-mode`:

```
output/
├── keys/
│   ├── KEY1.info.json
│   └── KEY2.info.json
├── conv/
│   └── filename.ext
├── custom_folders.txt
└── [downloaded files]
```

## Troubleshooting

### "Couldn't find download url" Errors

MediaFire occasionally changes their page structure. This tool has been updated to handle current MediaFire HTML patterns. If you encounter this error:

1. Ensure you're running the latest version
2. Try reducing threads: `--threads 1`
3. The file may have been removed from MediaFire

### Captchas

If MediaFire shows captchas:

1. Reduce thread count: `--threads 1`
2. Wait a few minutes and retry
3. Download files manually via browser

## License

This project is licensed under **GPL-3.0**. See [LICENSE](LICENSE) for details.

## Credits

- **Pyxia** - Original author ([gitgud.io/Pyxia/mf-dl](https://gitgud.io/Pyxia/mf-dl))
- **themadprogramer** - Tutorial and documentation contributions ([Data Horde](https://datahorde.org/))
- **Archive Team** - MediaFire archiving advocacy ([archiveteam.org](https://archiveteam.org/))
