"""Search WikiLeaks documents via their public search interface."""

import argparse
import sys
from datetime import datetime

import requests
from bs4 import BeautifulSoup


def search_wikileaks(search_term: str, timeout: int = 15) -> list[dict]:
    """Search WikiLeaks and return a list of result dicts.

    Args:
        search_term: The term to search for on WikiLeaks.
        timeout: Request timeout in seconds.

    Returns:
        A list of dicts, each containing title, link, excerpt,
        leak_label, thumbnail_url, created_date, and released_date.
    """
    url = f'https://search.wikileaks.org/?q={requests.utils.quote(search_term)}'

    try:
        response = requests.get(url, timeout=timeout)
        response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print(f'Error fetching search results: {e}', file=sys.stderr)
        return []

    soup = BeautifulSoup(response.text, 'html.parser')

    results = []
    for result in soup.find_all('div', {'class': 'result'}):
        title_element = result.find('a')
        if not title_element:
            continue

        title = title_element.get_text()
        link = title_element.get('href')

        excerpt_element = result.find('div', {'class': 'excerpt'})
        excerpt = excerpt_element.text.strip() if excerpt_element else ''

        leak_label_element = result.find('div', {'class': 'leak-label'})
        leak_label = leak_label_element.text.strip() if leak_label_element else ''

        thumbnail_element = result.find('img', {'alt': 'Plusd'})
        thumbnail_url = thumbnail_element['src'] if thumbnail_element else ''

        created_element = result.find('div', {'class': 'date'})
        created_date = (
            created_element.text.strip().split('\n\n')[0]
            if created_element
            else ''
        )

        results.append({
            'title': title,
            'link': link,
            'excerpt': excerpt,
            'leak_label': leak_label,
            'thumbnail_url': thumbnail_url,
            'created_date': created_date,
            'released_date': '',
        })

    return results


def main() -> None:
    parser = argparse.ArgumentParser(
        description='Search WikiLeaks documents.'
    )
    parser.add_argument(
        '--term',
        default='kissinger',
        help="Search term (default: 'kissinger')",
    )
    args = parser.parse_args()

    results = search_wikileaks(args.term)

    if not results:
        print(f'No results found for "{args.term}".')
        sys.exit(1)

    print(f'Search results for "{args.term}":')
    for i, result in enumerate(results, start=1):
        print(f'{i}. Title: {result["title"]}')
        print(f'   Link: {result["link"]}')
        print(f'   Excerpt: {result["excerpt"]}')
        print(f'   Leak Label: {result["leak_label"]}')
        print(f'   Thumbnail URL: {result["thumbnail_url"]}')
        print(f'   Created Date: {result["created_date"]}')
        print('---')


if __name__ == '__main__':
    main()
