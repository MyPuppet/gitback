# gitback
githack改良版本
https://github.com/lijiejie/GitHack 整个代码是复制liejiejie的githack.py
与之不同的地方在于以下几点：
* 添加了options可以选择线程大小，默认5线程
* 可以选择下载文件的大小，如果文件太大中间会断开，默认5M

在下载服务器上面代码的时候，目标服务器可能会设置重定向到某个页面，结果下载下来的文件内容都相同，
所以在302或者404重定向的时候会退出线程，继续下载其他的文件。
在第66行代码里有时候抛出read time out，至今不晓得肿么回事。
