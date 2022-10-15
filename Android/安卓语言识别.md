## Android集成百度语音识别怎么避坑？
首先先放一张集成失败的图（记得一定要用真机，因为它不支持VAD，我这里使用Pixel2）：![集成失败的截图](https://img-blog.csdnimg.cn/20200603111239593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
首先你去百度搜索“百度语音识别”，或者点击我下面的链接
[百度语音识别平台](https://ai.baidu.com/tech/speech?track=cp:ainsem%7Cpf:pc%7Cpp:chanpin-yuyin%7Cpu:yuyin-yuyinshibie-pinpai%7Cci:%7Ckw:10003653)
然后去创建一个应用，名字不要太雷同就好
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603111809104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
创建成功后你会得到自己应用的APPID和KEY
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603112007104.png)
现在需要下载它的SDK文件，点击下来的链接也可以
[SDK下载地址](https://ai.baidu.com/sdk#asr)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603112208795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
下载完解压后打开文件夹找到如图文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603112340926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603112357896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
里面包括了官方集成文档，虽然我自己试了五六次集成都失败，但我还是打不死的小强，通过不断查找方法终于找到解决的方法，也正好这个方法我在百度上是找不到的，所以想分享给大家。
话不多说直接新建一个工程：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603112845986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
先导入刚刚下载好的SDK模块
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603113048489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
找到文件中的core文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603113126444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603113133946.png)
导入的过程需要点时间，继续下一步，右键app找到模Module Settings
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603113611806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
这里从左往右看Dependencies > app > + > Moudule Dependency
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603113804733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060311381959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
打上勾，OK，APPLY/OK,会重新刷新项目
把Android切换为Project，找到settings.grade，有下面截图就代表成功了一半！！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603114056519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
**首先我们要清楚，导入别人的库和包一定要遵循别人提供的版本号和构建器的版本号，所现在需要把自己项目的各种版本号改成和core模块一样的版本号**
可以看下core的版本号先
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603114422742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
这里千万不要手贱升级
现在切换回Android目录，这是没有修改前的版本，我的版本都比较新，所以要改跟core一样的版本
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603114552212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
修改后的版本号，minSdkVersion这个可以不用改，改完后记得Sync Now,没有提示Sync Now(可以点击File > Sync project with Gradle File),不然没有修改成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200607202004721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603114806193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
还有一个地方比较坑的没改，可以看下官方文档给出的版本号
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603114945716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
Gradle的版本也是需要改的，可以看下我修改前的版本是4.0.0
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603115028396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
修改后都要刷新（Sync project）一遍项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603115208573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
现在前期的全部步骤都完成了，现在来到MainActivity.class文件改成如下:是把*AppCompatActivity*改成*ActivityMiniRecog*，最后安装软件就可以了。

```java
public class MainActivity extends ActivityMiniRecog(){
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603115349438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
没有报错就可以运行了，如果你是跟我的步骤做完的我可以保证你99%可以成功！
奉上成功的截图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603120529753.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
希望这篇文章可以帮到正在折腾的你，可以点个关注收藏喔！
没有许可禁止转载我的文章！
