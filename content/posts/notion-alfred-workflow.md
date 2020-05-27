---
title: A custom notion-alfred workflow
date: 2019-11-18
published: false
tags: ['productivity']
series: false
canonical_url: false
description: ''
---

In an attempt to work a little smarter, I've been investing some time into automating a few of my daily tasks and optimising my workflow for improved productivity.

When it comes to productivity strategies, the Eisenhower decision matrix is a personal favourite. In addition to personal progress, I've found it to be very effective when product managing in a cross-functional team environment. The strategy revolves around Eisenhower's saying

> What is important is seldom urgent and what is urgent is seldom important

The following graphic comes from an <a href="https://jamesclear.com/eisenhower-box" target="_blank">article</a> written by James Clear, but according to <a href="https://www.artofmanliness.com/articles/eisenhower-decision-matrix/" target="_blank">Art of Manliness</a>, the matrix was first popularised by Stephen Covey. Both of the aforementioned articles are good resources if the "Eisenhower Box" is a foreign concept to you.

![Eisenhower Box](https://res.cloudinary.com/tinavanschelt/image/upload/v1589728557/writing/eisenhower-box.jpg)

Enter two of my favourite productivity tools: Alfred and Notion. Alfred is a productivity tool for Mac, you could think of Alfred as Spotlight on steroids, and Notion is an all-in-one workspace destined for <a href="https://usefyi.com/notion-history/?utm_source=newsletter&utm_medium=email&utm_campaign=phnotion" target="_blank">greatness</a>.

I use the following Notion board to utilise the Eisenhower Decision Matrix and organise daily tasks.

![Eisenhower Board on Notion](https://res.cloudinary.com/tinavanschelt/image/upload/v1589728557/writing/eisenhower-board-notion.png)

Integrating Notion and Alfred required some effort, but here's the end result. (There is a slight delay, but bear in mind that I don't usually have Notion open when using the workflow, so the delay goes unnoticed.)

![Notion Alfred Workflow Demo](https://res.cloudinary.com/tinavanschelt/image/upload/v1589728557/writing/notion-alfred-workflow.gif)

<small>("Don't you know how busy and important I am" is a hat tip to one of my favourite artists, Tom Rosenthal, who is (a) a genius and (b) manages to capture the mindset of an entire generation in one song.)</small>

I wanted to be able to add and recategorise tasks directly from my desktop and started digging around the internet for an Alfred workflow. The wild wild web did not disappoint. <a href="https://kevinjalbert.com/integrating-notion-with-alfred/" target="_blank">Kevin Jalbert</a> published a very detailed <a href="https://github.com/kevinjalbert/alfred-notion" target="_blank">notion-alfred workflow</a> using <a href="https://github.com/jamalex/notion-py" target="_blank">notion-py</a>. The two repositories served as a valuable starting block for creating my own, as outlined below.

## First off, you'll need

<ol>
  <li><a href="https://www.alfredapp.com/" target="_blank">Alfred 3+</a> with a powerpack</li>
  <li>Python 3</li>
  <li>A Notion Board</li>
</ol>

## How to

<ol>
  <li>Open Alfred Preferences and create a new blank workflow</li>
  <li>Right-click on your newly created workflow and select "Open in Finder"</li>
  <li>Install `notion-py` in said folder from terminal `pip install notion`</li>
  <li>Configure script(s), here's a working script example:

```python
#!/Library/Frameworks/Python.framework/Versions/3.7/bin/python3

import sys
from notion.client import NotionClient # notion-py

client = NotionClient(token_v2="your-notion-token-v2")
view = client.get_collection_view("https://www.notion.so/username/xxx?v=xxx")

query = sys.argv[1]
row = view.collection.add_row()
row.name = query
row.status = "do" # the title of your board / list (so mine would be either do, decide, delegate, done)

print(query)
```

  <!-- mine is on github for reference (probably a good time to mention that I don't really have experience with Python -->
  </li>
  <li>
    Copy your Notion Token
    <img src="./images/get_notion_token.png" alt="Get Notion Token" />
  </li>
  <li>Copy Notion URL</li>
  (things to note)
  <li>Create Keyword and hook up script (screenshot)</li>
  <li>Add a push notification</li>
</ol>

## Debugging Tips

<ul>
  <li>Alfred has a debugger. It comes in real handy to track down tiny, obvious glitches in your code. 
    <img src="./images/alfred-debugger.png" alt="Get Notion Token" /></li>
  <li>Ensure your script is running from the terminal, outside of the terminal, <code>python3 your-script.py</code></li>
  <li>Ensure your python script is executable by running <code>chmod +x the-file-containing-your-script.py</code></li>
  <li>Don't forget the shebang at the top of your file <code>#!/Library/Frameworks/Python.framework/Versions/3.7/bin/python3</code></li>
</ul>
