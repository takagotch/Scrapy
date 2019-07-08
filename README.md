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

import scrapy
from myproject.items import MyItem

class MySpider(scrapy.Spider):
  name = 'example.com'
  allowed_domains = ['example.com']
  
  def start_requests(self):
    yield scrapy.Request('http://www.example.com/1.html', self.parse)
    yield scrapy.Request('http://www.example.com/2.html', self.parse)
    yield scrapy.Request('http://www.example.com/3.html', self.parse)

  def parse(self, response):
    for h3 response.xpath('//h3').getall():
      yield MyItem(title=h3)
    
    for href in response.xpath('//a/@href').getall():
      yield scrapy.Request(response.urljoin(href), self.parse)

import scrapy

class MySpider(scrapy.Spider):
  name = 'myspider'
  
  def __init__(self, category=None, *args, **kwargs):
    super(MySpider, self).__init__(*args, **kwargs)
    self.start_urls = ['http://www.example.com/categories/%s' % category]

import scrapy
 
class MySpider(scrapy.Spider):
  name = 'myspider'
  
  def start_requests(self):
    yield scrapy.Request('http://www.example.com/categories/%s' % self.category)
    
import scrapy

class TestItem(scrapy.Item):
  id = scrapy.Field()
  name = scrapy.Field()
  description = scrapy.Field()
  
import scrapy
from scrapy.spiders iport CrawlSpider, Rule
from scrapy.linkextractors import LinkExtractor

class MySpider(CrawlSpider):
  name = 'example.com'
  allowed_domains = ['example.com']
  start_urls = ['http://http://www.example.com']
  
  rules = (
    Rule(LinkExtractor(allow=('category\.php', ), deny=('subsection\.php'))),
    Rule(LinkExtractor(allow=('item\.php', )), callback='parse_item'),
  )
  
  def parse_item(self, response):
    self.logger.info('Hi, this is an item page! %s', response.url)
    item = scrapy.Item()
    item['id'] = response.xpath('//td@id="item_id"]/text()').re(r'ID: (\d+)')
    item['name'] = response.xpath('//td[@id="item_name"]/text()').get()
    item['description'] = response.xpath('//td[@id="item_description"]/text()').get()
    return item

itertag = 'product'

class YourSpider(XMLFeedSpider):
  namespaces = [('n', 'http://www.sitemaps.org/schemas/sitemap/0.9')]
  itertag = 'n:url'

from scrapy.spiders import XMLFeedSpider
from myproject.items import TestItem

class MySpider(XMLFeedSpider):
  name = 'example.com'
  allowed_domains = ['example.com']
  start_urls = ['http://www.example.com/feed.xml']
  iterator = 'iternodes'
  itertag = 'item'
  
  def parse_node(self, response, node)
    self.logger.info('Hi, this is a <%s> node!: %s', self.itertag, ''.join(node.getall()))
    
    item = TestItem()
    item['id'] = node.xpath().get()
    item['name'] = node.xpath('name').get()
    item['description'] = node.xpath('description').get()
    return item

from scrapy.spiders impport CSVFeedSpider
from myproject.items import TestItem

class MySpider(CSVFeedSpider):
  name = 'example.com'
  allowed_domains = ['example.com']
  start_urls = ['http://www.examplecom/feed.csv']
  delimiter = ';'
  quotechar = "'"
  headers = ['id', 'name', 'description']
  
  def parse_row(self, response, row):
    self.logger.info('Hi, this is a row!: %r', row)
    
    item = TestItem()
    item['id'] = row['id']
    item['name'] = row['name']
    item['description'] = row['description']
    return item

sitemap_rules = [('/product/', 'parse_product')]

from datetime import datetime
form scrapy.spiders import SitemapSpider

class FilteredSitemapSpider(SitemapSpider):
  name = ''
  allowed_domains = []
  sitemap_urls = []
  
  def sitemap_filter():
    for entry in entries:
      date_time = datetime.strptime(entry['lastmod'], '%Y-%m-%d')
      if date_time.year >= 2005:
        yield entry

from scrapy.spiders import SitemapSpider

class MySpider(SitemapSpider):
  sitemap_urls = []
  sitemap_rules = [
    ('/product/', 'parse_product'),
    ('/category/', 'parse_category'),
  ]
  
  def parse_product(self, response):
    pass
    
  def parse_category(self, response):
    pass

from scrapy.spiders import SitemapSpider

class MySpider(SitemapSpider):
  sitemap_urls = ['http://www.example.com/robots.txt']
  sitemap_rules = [
    ('/shop/', 'parse_shop')
  ]
  sitemap_follow = ['/sitemap_shop']
  
  def parse_shop(self, response):
    pass

from scrapy.spiders import SitemapSpider

class MySpider(SitemapSpider):
  sitemap_urls = ['http://www.example.com/robots.txt']
  sitemap_rules = [
    ('/shop/', 'parse_shop'),
  ]
  
  other_urls = ['http://www.example.com/about']
  
  def start_requests(self):
    requests = list(super(MySpider, self).start_requests())
    requests += [scrapy.Request(x, self.parse_other) for x in self.other_urls]
    return requests

  def parse_shop(self, response):
    pass
    
  def parse_other(self, response):
    pass
```

