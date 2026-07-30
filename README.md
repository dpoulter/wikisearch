# wikisearch

A Python script that scrapes and searches [WikiLeaks](https://search.wikileaks.org/) documents.

## Setup

```bash
pip install -r requirements.txt
```

## Usage

```bash
# Search with a specific term (default: 'kissinger')
python search_wikileaks.py --term "CIA"

# No arguments — defaults to 'kissinger'
python search_wikileaks.py
```

Each result includes:
- Title and link to the document
- Excerpt from the document
- Leak label (e.g. diplomatic, intelligence)
- Thumbnail URL (if available)
- Created date

## Notes

- This tool scrapes a public search interface; use responsibly.
- The script may break if WikiLeaks changes their HTML structure.