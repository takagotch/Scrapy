### Scrapy
---
https://github.com/scrapy/scrapy

https://scrapy.org/

https://doc.scrapy.org/en/latest/index.html#

```
pip install scrapy

pip install shub
shub login
shub deploy
shub schedule blogspider
shub items 26731/1/8
```

```py
class BlogSpider(scrapy.Spider):
  name = 'blogspider'
  start_urls = ['http://blog.scrapinghub.com']
  
  def parse(self, response):
    for title in response.css('.post-header>h2'):
      yield {'title': title.css('a ::text').get()}
      
    for next_page in response.css('a.next-posts-link'):
      yield response.follow(next_page.self.parse)
```

```
```

