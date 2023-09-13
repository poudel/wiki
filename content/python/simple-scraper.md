+++
title = "Generic scraper"
weight = 500
+++

# Generic scraper

I needed a way to scrap lists from several websites. The challenge is
that each site has its own unique HTML to present the list.

Something like scrapy would be an overkill for this project because
these are simple lists that are almost never big enough to require
pagination etc. My go-to for this type of job is `requests-html`. It's
simple but powerful library that builds on top of `requests` library.

But I don't want to write scraper for each website so I decided to build a
generic one that accepts configuration for each source.


## Setup {#setup}

```sh
pip install requests-html
```


## Scraper {#scraper}

```python
from dataclasses import dataclass
from typing import Optional
from requests_html import HTMLSession


@dataclass
class Source:
    name: str
    index_url: str
    render_html: bool

    # CSS selectors
    item_selector: str
    item_title_selector: str
    item_url_selector: Optional[str]



class Spider:

    def __init__(self, source: Source):
        self.source = source
        self.session = HTMLSession()

    def fetch_remote(self):
        index = self.session.get(self.source.index_url)

        # some websites need to run JS to render the list
        if self.source.render_html:
            index.html.render()
        return index

    def run(self):
        index = self.fetch_remote()
        list_elements = index.html.find(self.source.item_selector)

        result = []

        for e in list_elements:
            url = None
            title = None

            if self.source.item_url_selector:
                anchor = e.find(self.source.item_url_selector, first=True)
            else:
                anchor = e

            if anchor:
                url = anchor.absolute_links.pop()

            title_element = e.find(self.source.item_title_selector, first=True)
            if title_element:
                title = title_element.text

            if url or title:
                result.append((url, title))
        return result
```


## Test {#test}

```python
source = Source(
    name="Quotes Scraper",
    index_url="https://quotes.toscrape.com/",
    render_html=False,
    item_selector="div.quote",
    item_title_selector="span.text",
    item_url_selector=None,
)
quotes_spider = Spider(source)
print(quotes_spider.run())
```

This should give you a list of tuples containing the URL and title of
a quote.
