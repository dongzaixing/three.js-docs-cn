# 如何在本地运行

如果你只用程序化的几何体并且不会加载任何纹理贴图, 只需双击html文件即可运行. 你会发现浏览器地址栏中显示的`file:///yourFile.html`

## 加载外部文件

如果你要加载外部模型或者纹理贴图, 因为浏览器的[同源策略](http://en.wikipedia.org/wiki/Same_origin_policy), 此过程会抛出异常导致程序中断运行.

有两个方法可以解决:

1. 修改浏览器安全策略, 这将允许你直接访问`file:///yourFIle.html` 
2. 运行一个本地的web服务器. 运行你直接通过http协议访问文件. `http://localhost/yourFile.html`   

如果你使用方法1, 会让你的电脑变得易受攻击, 所以不推荐.

## 运行一个本地服务器

很多程序语言都有简单的http内置服务器. 他们不足以和生产环境服务器Apache或Nginx媲美, 但作为本地开发服务器还是很方便的.

#### Node.js 服务器

Node.js有一个简单的HTTP服务器包:

```bash
npm install http-server -g
```

使用当前目录运行:

```bash
http-server . -p 8000
```

#### Python服务器

如果你安装了python, 可以直接运行:

```py
# Python 2.x
python -m SimpleHTTPServer
```

```py
# Python 3.x
python -m http.server
```

这会将当前目录作为服务器根目录并使用8000端口

#### Ruby服务器

如果安装了Ruby,  则:

```ruby
ruby -r webrick -e "s = WEBrick::HTTPServer.new(:Port => 8000, :DocumentRoot => Dir.pwd); trap('INT') { s.shutdown }; s.start"
```

#### PHP服务器

在php5.4.0也内置了服务器:

```bash
php -S localhost:8000
```

#### Lighttpd

Lighttpd是个很轻量的web服务器. 和上面的服务器不同, Lighttpd是个功能齐全的生成环境服务器.

使用homebrew安装:

```bash
brew install lighttpd
```

在你想作为服务器根目录的地方创建配置文件lighttpd.conf

在lighttpd.conf中修改server.document-root为你想作为服务器根目录的路径

使用`lighttpd -f lighttpd.conf` 运行

 

## 修改本地的安全策略

#### safari

Advanced --&gt; show develop menu, 然后在develop菜单中, 选择 'Disabled local file restrictions'

#### Chrome

```bash
open /Applications/Google\ Chrome.app --args --allow-file-access-from-files
```

#### Firefox

地址栏中输入`about:config`

找到`security.fileuri.strictoriginpolicy`  并设置为false







