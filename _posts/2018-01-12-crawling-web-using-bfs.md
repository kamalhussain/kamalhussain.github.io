---
layout: post
title:  "Crawling web using BFS"
date:   2018-01-12 11:44:00
categories: programming
permalink: /crawling-web-using-bfs/
---

You can build a simple web crawler while learning how the "breadth first search(BFS)" works for the 
graph traversal. Following code demonstrates a simple web crawler that relies on BFS. You can also do the same 
using "depth first search (DFS)" BFS works better in most situations.

```python
from collections import deque
from bs4 import BeautifulSoup

import urllib.request
import re

queue = deque([])
visited = {}
root_node = "https://www.google.com"

queue.append(root_node)
visited[root_node] = 1

def crawl_web():

    while len(queue):

        node = queue.popleft()
        print("processing node: %s " % (node))

        try:
            with urllib.request.urlopen(node) as response:
                html_page = response.read()

        except urllib.error.URLError as e:
            print("oops! error parsing the page %s" % (node))
            print(e)
            continue

        soup = BeautifulSoup(html_page, 'html.parser')

        for link in soup.findAll('a', attrs = {'href': re.compile("^http://")}):
            link_href = link.get('href')

            if link_href in visited:
                print("already visited, skipping %s " % (link_href))

            else:
                visited[link] = 1
                queue.append(link_href)


crawl_web()
```

