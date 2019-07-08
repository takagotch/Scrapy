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

```py
from scrapy.mail import MailSender
mailer = MailSender()

mailer = MailSender.from_settings(settings)

mailer.send(to=["someone@example.com"], subject="Some subject", body="Some body", cc=["another@example.com"])s

def parse_page(self, response):
  if 'Bandwidth exceeded' in response.body:
    raise CloseSpider('bandwidth_exceeded')
    

class MySpider(scrapy.Spider):
  name = 'myspider'
  
  def start_requests(self):
    return [scrapy.FromRequest("http://www.example.com/login",
      formdata={'user': 'john', 'pass': 'secret'},
      callback=self.logged_in)]
      
  def logged_in(self, response):
    pass

import scrapy

class MySpider(scrapy.Spider):
  name = 'example.com'
  allowed_domains = ['example.com']
  start_urls = [
    'http://www.example.com/1.html',
    'http://www.example.com/2.html',
    'http://www.example.com/3.html'
  ]
  
  def parse(self, response):
    self.logger.info('A response from %s just arrived!', response.url)

import scrapy

class MySpider(scrapy.Spider):
  name = 'example.com'
  allowed_domains = ['example.com']
  start_urls = [
    'http://www.example.com/1.html',
    'http://www.example.com/2.html',
    'http://www.example.com/3.html'
  ]

  def parse(self, response):
    for h3 in response.xpath('//h3').getall():
      yield {"title": h3}
      
    for href in response.xpath('//a/@href').getall():
      yield scrapy.Request(response.urljoin(href), self.parse)
```

