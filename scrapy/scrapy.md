# Scrapy

## scrapy shell to get the content

``` python
fetch("https://www.myschool.edu.au/school/50503")
sel.xpath('//*[@id="body-area"]/div[2]/section[1]/div[1]/ul/li[1]/div[2]').extract()
```

Or we can

> `scrapy shell 'http://quotes.toscrape.com/'`

## Create a project

> `scrapy startproject quotes_spider`

## Gen spider belong to this project

> `cd quotes_spider`
> `scrapy genspider example example.com`
> `scrapy genspider quotes quotes.toscrape.com`
> `scrapy genspider myschool www.myschool.edu.au/school/50503`

## List spider

> `scrapy list`

## Edit spider

```python
# -*- coding: utf-8 -*-
import scrapy

class MyschoolSpider(scrapy.Spider):
    name = 'myschool'
    allowed_domains = ['www.myschool.edu.au/school/50503']
    start_urls = ['http://www.myschool.edu.au/school/50503/']

    def parse(self, response):
      icsea_score = response.xpath('//*[@id="body-area"]/div[2]/section[1]/div[1]/ul/li[1]/div[2]/text()').extract()
      school_name = response.xpath('/html/body/div[1]/div/div[1]/div[1]/h1/text()').extract()

      yield {'School_Name': school_name, 'ICSEA_Score': icsea_score}
```

## Run scrapy spider

> `scrapy crawl myschool`

## Get result

```bash
2018-11-07 00:21:31 [scrapy.core.engine] INFO: Spider opened
2018-11-07 00:21:31 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
2018-11-07 00:21:31 [scrapy.extensions.telnet] DEBUG: Telnet console listening on 127.0.0.1:6023
2018-11-07 00:21:31 [scrapy.downloadermiddlewares.redirect] DEBUG: Redirecting (301) to <GET https://www.myschool.edu.au/school/50503/> from <GET http://www.myschool.edu.au/school/50503/>
2018-11-07 00:21:31 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.myschool.edu.au/school/50503/> (referer: None)
2018-11-07 00:21:31 [scrapy.core.scraper] DEBUG: Scraped from <200 https://www.myschool.edu.au/school/50503/>
{'School_Name': [u'Glenroy Private, Glenroy, VIC'], 'ICSEA_Score': [u'929']}
2018-11-07 00:21:31 [scrapy.core.engine] INFO: Closing spider (finished)
```

## Find Xpath

1. Open the web page in Google Chrome.
2. Select the text portion you want to extract.
3. Right-click, and select "Inspect".
4. Select the HTML code you need, and select "Copy" and then "Copy XPath".
5. Paste the XPath to your code, test, and edit it, if necessary.
6. Note that this method copy the "id" but you can change it to the "class" of the same portion if that will work better.

![xpath](../images/scrapy/CopyXPath.png)

## Current level of xpath

> `quote.xpath('.//a')``

## Export to csv / json / xml

> `scrapy crawl quotes -o items.csv`
> `scrapy crawl quotes -o items.json`
> `scrapy crawl quotes -o items.xml`

## Scrapy Architecture

* setting file:  scrapy.cfg
* `project_folder/__init__.py`: mark python package directory
* `project_folder/items.py`: define return/yield in items.py
  * If we need return data have some specific order
* `project_folder/pipelines.py`: is going to be used when receiving an item and performing an action over it
  * need to modify `project_folder/settings.py` to enable pipeline
* `project_folder/settings.py`
  * `ROBOTSTXT_OBEY`
  * `ITEM_PIPELINES`
  * `DOWNLOAD_DELAY`
  * Can define it with `scrapy crawl quotes -s DOWNLOAD_DELAY=5`
  * `DEFAULT_REQUEST_HEADERS`
  * `USER_AGENT`

## Don't get be banned

* In the file settings.py activate the option DOWNLOAD_DELAY or you can do that manually in your code through sleeping a for a random number of seconds. If you use sleep and random, here is a code snippet:

    ```python
    from time import sleep
    import random

    # Your code here
    ...

    sleep(random.randrange(1,3))
    ```

* In the file settings.py activate the option USER_AGENT like the following, or any Chrome or Firefox user agent [here](http://www.useragentstring.com/pages/useragentstring.php). Defining a user agent let you look more like a browser used by a real person, not an automatic robot.

  * > `USER_AGENT = "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1"`

* Find external proxies and rotate IP addresses while scraping. You can use the package scrapy-proxies for the purpose.

  * > https://github.com/aivarsk/scrapy-proxies

* For professional work, consider using `ScrapingHub.com` to host your scrapers - it offers a free limited plan.

## Recommendations Before Web Scraping:

* Before web scraping, it is highly recommended to search for an `API for the website` you want to get data from.

* Before web scraping, it is highly recommended you read the `Terms and Conditions` of the website. Some websites clearly mention prohibiting web scraping without permission, or mention some legal or copyright aspects related to the use of its data.

* Before web scraping, employ common sense! Some web scraping or other robot activities are obviously `illegal` if they cause any direct or indirect damage to the company owning data or its customers.

* Before web scraping, prepare your code to be "polite": do not `unnecessarily disable robots.txt` of the website; space out your requests a bit so that you do not hammer the site's server; and it is better to run your spiders during off-peak traffic hours of the website.

