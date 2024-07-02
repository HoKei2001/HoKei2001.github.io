---
title: Python 异步函数与协程
date: 2024-07-02 12:00:00 +0000
categories: [Crawler, Crawlee]
tags: [python, Crawlee,async]     # TAG names should always be lowercase
---

## A crawler function
下面是一个crawlee python 提供的爬虫示例
```python
async def crawlee(url) -> str:
    # Create a HttpCrawler instance and provide a starting requests
    crawler = HttpCrawler()

    # Define a handler for processing requests
    @crawler.router.default_handler
    async def request_handler(context: HttpCrawlingContext) -> None:
        # Crawler will provide a HttpCrawlingContext instance,
        # from which you can access the request and response data
        data = {
            'url': context.request.url,
            'status_code': context.http_response.status_code,
            'headers': dict(context.http_response.headers),
            'response': context.http_response.read().decode()#[:1000],
        }
        # Extract the record and push it to the dataset
        await context.push_data(data)

    # Add first URL to the queue and start the crawl
    await crawler.run([url])

    return 
```


这是一个使用Python定义的异步函数。下面是对其各部分的详细解释：

1. **`async def`**:
   - `async` 关键字用于定义一个异步函数。异步函数在调用时会返回一个协程对象，这个对象可以用来控制函数的执行。
   - `def` 关键字用于定义一个函数。

2. **`crawlee`**:
   - 这是函数的名字。

3. **`url`**:
   - `url` 是该函数的参数。这个参数代表一个传入的URL地址。

4. **`-> None`**:
   - 这是一个函数返回类型提示。它表明这个函数不返回任何值（即返回类型是 `None`）。
   - 使用箭头符号 `->` 后跟返回类型是 Python 3.5 引入的函数注解特性的一部分，目的是提高代码的可读性和可维护性。


