# gitback

https://github.com/lijiejie/GitHack 整个代码是复制liejiejie的githack.py
与之不同的地方在于以下几点：
* 添加了options可以选择线程大小，默认5线程
* 可以选择下载文件的大小，如果文件太大中间会断开，默认5M
* 如果直接采用socket的read解压缩或者requests.content，压缩文件太大的情况下会断开链接，导致下载文件不全。所以这个采用的方法是先下载文件，然后解压缩到目标文件。

different with githack:
* add the number of threads options, defalut is 5 thread.
* when you download the file, you can chose the bigest size, default is 5M.
* if you use reponse.raw.read(), when the file you download is too big, it will blocking until the download is break. so I download the zlib file first, then `zlib.decompress(fd.read())` 

在下载服务器上面代码的时候，目标服务器可能会设置重定向到某个页面，结果下载下来的文件内容都相同，
所以在302或者404重定向的时候会退出线程，继续下载其他的文件。

when you download the file, if the server setup redirect, it will download wrong file(not the zlib file).
so i use `requests.get(url, allow_redirects=False)`, only download the file when the `status_code == 200`.
so you must be sure the right download link, for example: `http://www.vulnerability.com` is different with `http://vulnerability.com`, the sample way is open your browser, enter the `vulnerability/.git/config` to see whether it can download the config file.


## 用法示例 ##
    ./gitback.py -t 10 -s 10 http://website.com
## 注意事项 ##
* 因为程序在requests里面设置了不允许跳转，所以`./gitback.py http://website.com` 和 `./gitback.py http://www.website.com` 结果是不同的。最简单的就是打开浏览器，看看那个网址能访问`http://weisite.com/.git/config`，能访问的就可以使用脚本下载。
* 如果要设定代理的话，直接在requests里面添加即可`proxies = {'http': 'http://127.0.0.1:1080'}`

## Thanks 
* 程序基本是从lijiejie的[githack](https://github.com/lijiejie/GitHack)复制过来的，我只是在使用的时候发现小问题。
* 另外使用了sbp的gin解析库[gin - a Git index file parser](https://github.com/sbp/gin)。
