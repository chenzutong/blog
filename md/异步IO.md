## 协程
协程，又称微线程，纤程。Coroutine  
协程在执行过程中，在子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。  

Python对协程的支持是通过generator实现的。

在generator中，我们不但可以通过for循环来迭代，还可以不断调用next()函数获取由yield语句返回的下一个值。

但是Python的yield不但可以返回一个值，它还可以接收调用者发出的参数。
```buildoutcfg
def consumer():
    r = ''  # 2
    while True:
        n = yield r  # 3....8
        if not n:
            return      # 4
        print('[CONSUMER] Consuming %s...' % n)     # 6
        r = '200 OK'        # 7


def produce(c):
    c.send(None)  # 1
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)     # 5
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s ' % r)        # 9
    c.close()


c = consumer()
produce(c)

输出结果：
[PRODUCER] Producing 1...
[CONSUMER] Consuming 1...
[PRODUCER] Consumer return: 200 OK 
[PRODUCER] Producing 2...
[CONSUMER] Consuming 2...
[PRODUCER] Consumer return: 200 OK 
[PRODUCER] Producing 3...
[CONSUMER] Consuming 3...
[PRODUCER] Consumer return: 200 OK 
[PRODUCER] Producing 4...
[CONSUMER] Consuming 4...
[PRODUCER] Consumer return: 200 OK 
[PRODUCER] Producing 5...
[CONSUMER] Consuming 5...
[PRODUCER] Consumer return: 200 OK 
```

## asyncio
asyncio是标准库，内置了对异步IO的支持  
asyncio的编程模型就是一个消息循环。  
我们从asyncio模块中直接获取一个EventLoop的引用，然后把需要执行的协程扔到EventLoop中执行，就实现了异步IO。  
```buildoutcfg
import threading
import asyncio


# @asyncio.coroutine把一个generator标记为coroutine类型
@asyncio.coroutine
def hello1():
    print('Hello1 world! (%s)' % threading.currentThread())
    # 异步调用asyncio.sleep(1),也是一个coroutine,线程不会等待而是直接中断并执行下一个消息循环。
    yield from asyncio.sleep(1)
    print('Hello1 again! (%s)' % threading.currentThread())

@asyncio.coroutine
def hello2():
    print('Hello2 world! (%s)' % threading.currentThread())
    yield from asyncio.sleep(1)
    print('Hello2 again! (%s)' % threading.currentThread())

# 获取EventLoop:
loop = asyncio.get_event_loop()
tasks = [hello1(), hello2()]
# 执行coroutine
loop.run_until_complete(asyncio.wait(tasks))
loop.close()

输出结果：
Hello1 world! (<_MainThread(MainThread, started 139816879376192)>)
Hello2 world! (<_MainThread(MainThread, started 139816879376192)>)
（等待1秒）
Hello1 again! (<_MainThread(MainThread, started 139816879376192)>)
Hello2 again! (<_MainThread(MainThread, started 139816879376192)>)
```

```buildoutcfg
import asyncio

@asyncio.coroutine
def wget(host):
    print('wget %s...' % host)
    connect = asyncio.open_connection(host, 80)
    reader, writer = yield from connect
    header = 'GET / HTTP/1.0\r\nHost: %s\r\n\r\n' % host
    writer.write(header.encode('utf-8'))
    yield from writer.drain()
    while True:
        line = yield from reader.readline()
        if line == b'\r\n':
            break
        print('%s header > %s' % (host, line.decode('utf-8').rstrip()))
    # Ignore the body, close the socket
    writer.close()

loop = asyncio.get_event_loop()
tasks = [wget(host) for host in ['www.sina.com.cn', 'www.sohu.com', 'www.163.com']]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()

输出结果：
wget www.sohu.com...
wget www.sina.com.cn...
wget www.163.com...
www.163.com header > HTTP/1.1 200 OK
www.163.com header > Date: Mon, 30 Dec 2019 02:46:09 GMT
www.163.com header > Content-Type: text/html; charset=GBK
www.163.com header > Connection: close
www.163.com header > Expires: Mon, 30 Dec 2019 02:47:03 GMT
www.163.com header > Server: nginx
www.163.com header > Cache-Control: no-cache,no-store,private
www.163.com header > Age: 26
www.163.com header > Vary: Accept-Encoding
www.163.com header > X-Ser: BC51_dx-lt-yd-shandong-jinan-5-cache-6, BC18_dx-lt-yd-shandong-jinan-5-cache-6, BC79_dx-guangdong-jiangmen-7-cache-4
www.163.com header > cdn-user-ip: 240e:f8:5102:d157:f858:e027:fb03:3b48
www.163.com header > cdn-ip: 240e:97e:2000:c100:0:1:2:22
www.163.com header > X-Cache-Remote: HIT
www.163.com header > cdn-source: baishan
www.sina.com.cn header > HTTP/1.1 302 Moved Temporarily
www.sina.com.cn header > Server: nginx
www.sina.com.cn header > Date: Mon, 30 Dec 2019 02:46:09 GMT
www.sina.com.cn header > Content-Type: text/html
www.sina.com.cn header > Content-Length: 138
www.sina.com.cn header > Connection: close
www.sina.com.cn header > Location: https://www.sina.com.cn/
www.sina.com.cn header > X-Via-CDN: f=edge,s=ctc.jiangxi.union.31.nb.sinaedge.com,c=240e:f8:5102:d157:f858:e027:fb03:3b48;
www.sina.com.cn header > X-Via-Edge: 157767396936941f803fb400036004e7d4a55
www.sohu.com header > HTTP/1.1 200 OK
www.sohu.com header > Content-Type: text/html;charset=UTF-8
www.sohu.com header > Connection: close
www.sohu.com header > Server: nginx
www.sohu.com header > Date: Mon, 30 Dec 2019 02:46:04 GMT
www.sohu.com header > Cache-Control: max-age=60
www.sohu.com header > X-From-Sohu: X-SRC-Cached
www.sohu.com header > Content-Encoding: gzip
www.sohu.com header > FSS-Cache: HIT from 9921510.18506736.10599785
www.sohu.com header > FSS-Proxy: Powered by 2450292.3564414.3128453
```
asyncio提供了完善的异步IO支持；

异步操作需要在coroutine中通过yield from完成；

多个coroutine可以封装成一组Task然后并发执行

## async/await
为了简化并更好地标识异步IO，从Python 3.5开始引入了新的语法async和await，可以让coroutine的代码更简洁易读。

请注意，async和await是针对coroutine的新语法，要使用新的语法，只需要做两步简单的替换：

1. 把@asyncio.coroutine替换为async；
2. 把yield from替换为await。

如：
```buildoutcfg
@asyncio.coroutine
def hello():
    print("Hello world!")
    r = yield from asyncio.sleep(1)
    print("Hello again!")

改成：
async def hello():
    print("Hello world!")
    r = await asyncio.sleep(1)
    print("Hello again!")
```
