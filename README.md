# Python-Crawler-Tutorial-Cookie
Cookie的使用
**Cookie，指某些网站为了辨别用户身份、进行session跟踪而储存在用户本地终端上的数据（通常经过加密）**  
## 1.Opener
当你获取一个URL你使用一个opener(一个urllib2.OpenerDirector的实例)。在前面，我们都是使用的默认的opener，也就是urlopen。它是一个特殊的opener，可以理解成opener的一个特殊实例，传入的参数仅仅是url，data，timeout。

如果我们需要用到Cookie，只用这个opener是不能达到目的的，所以我们需要创建更一般的opener来实现对Cookie的设置。  

## 2.Cookielib
cookielib模块的主要作用是提供可存储cookie的对象，以便于与urllib2模块配合使用来访问Internet资源。Cookielib模块非常强大，我们可以利用本模块的CookieJar类的对象来捕获cookie并在后续连接请求时重新发送，比如可以实现模拟登录功能。该模块主要的对象有CookieJar、FileCookieJar、MozillaCookieJar、LWPCookieJar。

它们的关系：CookieJar —-派生—->FileCookieJar  —-派生—–>MozillaCookieJar和LWPCookieJar

### (1).获取Cookie保存到变量
首先，我们先利用CookieJar对象实现获取cookie的功能，存储到变量中  
`import urllib2`</br>
`import cookielib`</br>
`#声明一个CookieJar对象实例来保存cookie`</br>
`cookie = cookielib.CookieJar()`</br>
`#利用urllib2库的HTTPCookieProcessor对象来创建cookie处理器`</br>
`handler=urllib2.HTTPCookieProcessor(cookie)`</br>
`#通过handler来构建opener`</br>
`opener = urllib2.build_opener(handler)`</br>
`#此处的open方法同urllib2的urlopen方法，也可以传入request`</br>
`response = opener.open('http://www.baidu.com')`</br>
`for item in cookie:`</br>
 `   print 'Name = '+item.name`</br>
 `   print 'Value = '+item.value`</br>
 
 ### (2).保存Cookie到文件
`import cookielib`</br>
`import urllib2`
 
`#设置保存cookie的文件，同级目录下的cookie.txt`</br>
`filename = 'cookie.txt'`</br>
`#声明一个MozillaCookieJar对象实例来保存cookie，之后写入文件`</br>
`cookie = cookielib.MozillaCookieJar(filename)`</br>
`#利用urllib2库的HTTPCookieProcessor对象来创建cookie处理器`</br>
`handler = urllib2.HTTPCookieProcessor(cookie)`</br>
`#通过handler来构建opener`</br>
`opener = urllib2.build_opener(handler)`</br>
`#创建一个请求，原理同urllib2的urlopen`</br>
`response = opener.open("http://www.baidu.com")`</br>
`#保存cookie到文件`</br>
`cookie.save(ignore_discard=True, ignore_expires=True)`</br>

### (3).从文件中获取Cookie并访问
`import urllib2`
 
`#创建MozillaCookieJar实例对象`</br>
`cookie = cookielib.MozillaCookieJar()`</br>
`#从文件中读取cookie内容到变量`</br>
`cookie.load('cookie.txt', ignore_discard=True, ignore_expires=True)`</br>
`#创建请求的request`</br>
`req = urllib2.Request("http://www.baidu.com")`</br></br>
`#利用urllib2的build_opener方法创建一个opener`</br>
`opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))`</br>
`response = opener.open(req)`</br>
`print response.read()`</br>


