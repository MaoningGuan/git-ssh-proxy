## 问题描述：
目前，我们国内使用git clone, git push等速度是比较慢的，而我们通常是使用SSH协议。接下来，将介绍如何给git SSH的方式设置代理。
## 环境配置：
```python
Windows 10 64bit
```
## 解决方法：
前提条件是你的电脑已经开启了IP代理，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714184229665.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YW5tYW9uaW5n,size_16,color_FFFFFF,t_70)
此处，我自己已经在电脑用shadowsocks开启了VPN代理。
查看代理开启结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714184501569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1YW5tYW9uaW5n,size_16,color_FFFFFF,t_70)
我们可以看到代理已经开启成功了，而且代理走的是：127.0.0.1:1080（这里在设置config文件的时候用到）
接下来正式设置git ssh代理。
（1）去到以下路径：

```python
C:\Users\18127\.ssh
```
（2）新建config文件，文件名即为：config
（3）config文件的内容如下：

```python
# 这里的 -a none 是 NO-AUTH 模式，参见 https://bitbucket.org/gotoh/connect/wiki/Home 中的 More detail 一节
ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p

Host github.com
  User git
  Port 22
  Hostname github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\18127\.ssh\id_rsa"
  TCPKeepAlive yes

Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\18127\.ssh\id_rsa"
  TCPKeepAlive yes
```
到此，就可以成功设置git ssh代理了。
（4）查看代理设置的效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200714185021959.png)
这个速度跟你电脑本身的VPN代理下载速度有关，我自己的速度由几k/s变成了200多k/s。
