# Scrapy

## scrapy shell to get the content

``` python
fetch("https://www.myschool.edu.au/school/50503")
sel.xpath('//*[@id="body-area"]/div[2]/section[1]/div[1]/ul/li[1]/div[2]').extract()
```

## Commend line

### Create a project

> `scrapy startproject quotes_spider`

### Gen project code

> `cd quotes_spider`
> `scrapy genspider example example.com`
> `scrapy genspider quotes quotes.toscrape.com`
> `scrapy genspider myschool www.myschool.edu.au/school/50503`

### List task

> `scrapy list`

### Edit scrapyer

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

### Run scrapy task

> `scrapy crawl myschool`

### Get result

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
