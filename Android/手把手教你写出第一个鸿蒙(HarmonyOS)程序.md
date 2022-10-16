# 手把手教你写出第一个鸿蒙(HarmonyOS)程序

运行完第一个APP，真的有点鸿蒙（好懵）的感觉？？就这？？

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914021159772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

鸿蒙的开源地址：[鸿蒙开源地址](https://openharmony.gitee.com)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914013004766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

###  一、打开官网[HarmonyOS](https://developer.harmonyos.com/cn/home)，源码编译请下载： [源码编译器](https://device.harmonyos.com/cn/ide)，开发应用请下载：[HUAWEI DevEco Studio](https://developer.harmonyos.com/cn/develop/deveco-studio)，开发环境需要配置好JDK、Node.js，这个百度自己找！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914013502514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

### 　二、安装完成后，需要设置下SDK的安装位置。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914014049216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914014147906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

地址是可以修改成自己的路径，platforms 和 tools的勾全部打上！！！然后点击确定。
### 　三、创建第一个项目步骤如下图：
它有支持三种类型的应用： TV 设备应用、Wearable 可穿戴设备应用、Lite Wearable 可穿戴设备(Lite)应用，我选择创建TV项目 **（Java）**，选择（list）模板。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914014640792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091401522956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914015451523.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)
###  四、因为它这个IDE默认就是需要下载gradle5.4.1的版本的，而且下载又慢，就算开梯子也一样慢，就算去下载一个本地版的也是不行的，那现在需要改下配置！！改好之后关掉IDE重新打开！

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091401580529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914015902683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

看到这样就是成功了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020045406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

###  五、现在是开始配置华为的虚拟机，请把你的默认浏览器改成微软或者火狐，谷歌的应该有限制用不了！！！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020202355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020242516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020330430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020435302.png#pic_center)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020647570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

**应该是映射式的虚拟机，所以基本不占用内存，小内存电脑的福音！！！**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020735972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914020804671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

###  六、开始运行程序，按照这个步骤肯定是可以运行成功的！！！恭喜你开始正式运行鸿蒙第一个APP啦！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914021110414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914021143182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)
