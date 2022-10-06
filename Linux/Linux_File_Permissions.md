# Linux文件权限
Linux是一个多用户操作系统，它以“所有权”和“权限”的概念来保​​证文件的安全。

## ```Users(用户)```和```groups（组）```
- Linux 使用用户的概念来区分使用计算机的各种人。
- 每个用户都有一些与之关联的属性，例如用户 ID 和主目录。
- 为了更轻松地管理用户，您可以将用户添加到“组”中。
- 一个组可以有零个或多个用户。特定用户与“默认组”相关联，也可以是系统上其他组的成员。
- 如果要查看系统上的用户，可以/etc/passwd 通过运行以下命令查看文件：
```bash
$ cat /etc/passwd
```
- 在这里，每一行都包含用户的详细信息。具体来说，您可以在每一行的开头看到用户名，在第一行之前```:```。
- 同样，您可以```/etc/group```通过运行以下命令查看文件来查看系统上的组：
```bash
$ cat /etc/group
```
- 同样，每一行都包含用户的详细信息。每行的第一部分包含组名。运行这些命令时，您会注意到还有许多其他用户和组不是您创建的。这些是系统用户和组，用于安全地运行后台进程。
  
## ```Ownership（所有权）``` 和 ```permissions（权限）```
在 Linux 中，我们使用权限来控制用户可以对文件或目录执行的操作。Linux 使用三种类型的权限：
- **读取（r）**：对于文件，读取权限允许用户查看文件的内容。对于目录，读取权限允许用户查看存储在其中的文件和其他目录的名称。
- **写 (w)**：对于文件，写权限允许用户修改和删除文件。对于目录，写权限允许用户修改其内容（创建、删除和重命名其中的文件）。但是，除非同时启用了执行权限，否则此权限对目录没有影响。
- **执行 (x)**：当在文件上设置时，写权限允许它被执行。但是，除非还启用了读取权限，否则该权限对文件没有影响。另一方面，对于目录，写入权限允许用户进入目录（带有cd）并查看其中的文件和目录的元数据（如文件权限）。

文件和目录只能由单个用户和单个组拥有。每当用户创建文件或目录时，该文件由用户和用户的默认组“拥有”。

对于任何文件或目录，都有三种类型的“权限类”。您可以为这些类分配不同的权限，从而控制谁可以访问和修改文件。权限类如下：

- **用户**：此类中的权限会影响文件的所有者。
- **组**：此类中的权限影响拥有该文件的组。但是，如果所有者用户在该组中，则应用“用户”权限，而不是组权限。
- **其他**：此类中的权限会影响系统上的所有其他用户。

## 查看权限
查看给定目录中文件权限的最简单方法是运行：
```bash
$ ls -l <目标路径>
```
如果要查看当前目录中的权限，请在末尾省略目录名称：

```bash
$ ls -l
```
例如，我们使用"目录"显示目录的内容。输出以列的形式显示，如下所示：```ls -l /etc```

![image](https://www.booleanworld.com/wp-content/uploads/2018/04/permissions-annotated.png?ezimgfmt=ng:webp/ngcb20)

由上图可知"文件模式"、"所有者"、"组"的具体信息。
而"文件模式"列以紧凑的方式显示文件类型和权限，如下所示：

![image](https://www.booleanworld.com/wp-content/uploads/2018/04/classes.png?ezimgfmt=ng:webp/ngcb20)

- 第一个字符表示文件的类型。一个正常的文件代表与一个```-```,目录与一个```d```和象征性的链接```l```。例如，在截图中```ls```，您可以说```acpi```这是一个目录，而```adduser.conf```是一个文件。
- 然后，我们拥有相应的"用户"、"组"和"其他"类的权限。```rwx```表示这些类是否具有读取、编写和执行权限。如果```rwx```中出现一个```-```，则意味着相应的权限被禁用了。
- 现在，思考下```at.deny```这个文件，它的"文件模式"是```-rw-r--r--```。这意味着"root"用户拥有读取、编写的权限。"root"组中的用户以及任何"其他"用户只拥有读取的权限。

### 表示权限：数字和符号模式
上面的例子已经介绍如何使用```rwx```表示权限，这种方式叫做"符号模式"，但是，还有一种使用数字（称为"数字模式"）来表示权限的附加方式。

例如，请考虑上面讨论过的```acpi```目录。此目录的符号模式是```rwxr-xr-x``` 。现在，为了获得等效的数字模式，可以采取"用户"、"组"和"其他"的单独权限。如果启用了权限，就放 ```1``表示，如果禁用了，就放 ```0```表示。通过这样做，我们获得一个二进制号码，并将其转换为十进制。因此，对于目录，您可以获得如下所示的符号模式：

![image](https://www.booleanworld.com/wp-content/uploads/2018/04/43260_115.png?ezimgfmt=ng:webp/ngcb20)

因此，这样获得的数字模式为```755```。同样，如果数字模式（如 ```644```），您可以将其转换为二进制，每一个数字代表一组二进制，然后放入对应的字母```rwx```中,如结果为```1```就表示启用权限就要在对应的位置写上对应权限的字母，如果是```0```就表示禁用了权限，在对应位置写上```-```。可以参考下图的分析：

![image](https://www.booleanworld.com/wp-content/uploads/2018/04/sym2num1.png?ezimgfmt=ng:webp/ngcb20)


因此，这样获得的符号模式为```rw-r--r--```,这就表示所有者有读写权限，而组用户和其他用户只要读权限。

#### 如果不熟悉转换二进制的方式解读权限，可以参考下图数字与权限的解读：


| 二进制数 | ```rwx``` |     权限描述     |
|:--------:|:---------:|:----------------:|
|    0     | ```---``` |      无权限      |
|    1     | ```--x``` |       执行       |
|    2     | ```-w-``` |        写        |
|    3     | ```-wx``` |    编写和执行    |
|    4     | ```r--``` |        读        |
|    5     | ```r-x``` |    阅读和执行    |
|    6     | ```rw-``` |       读写       |
|    7     | ```rwx``` | 阅读、编写和执行 |

### 更改权限：chmod命令
创建文件或目录时，您可能需要更改某些权限。您可以通过```chmod```命令做到这一点。

首先，先创建一个用于测试目的的文件。
```bash
$ touch file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ touch file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rw-r--r-- 1 kizaiwsl2 kizaiwsl2 0 9月  16 21:56 file.txt
```
可以使用```ls -l file.txt```来查看该文件的权限，由结果可知该文件的权限是```rw-r--r--```。

现在，假设您要添加所有用户的执行权限 - 所有者、组和其他用户。由于我希望为所有用户添加执行权限，因此我使用```a+x```实现这个功能：
```bash
$ chmod a+x file.txt
```
```a```代表是改变所有用户的权限,```+```表示需要添加权限,```x```表示执行权限的意思，所以组合起来的意思是：给目标文件的所有用户添加执行的权限。
```bash
kizaiwsl2@KIZAI-DESKTOP:~$ chmod a+x file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxr-xr-x 1 kizaiwsl2 kizaiwsl2 0 9月  16 21:56 file.txt
```

其实默认情况下```chmod```执行的对象是所有用户，你可以把```a```去掉，用下面的命令也同样能实现相同的功能。
```bash
$ chmod +x file.txt
```

```chmod```还可以更细致改变文件的权限，```u g o```这些字母分别表示“用户”、“组”、“其他用户”，同样它也能使用```-```去掉某一个权限，还是以file.txt的文件为例，我现在不想其他用户拥有读和执行的权限，组里面去掉读的权限，可以使用以下的命令：注意我使用```,```来分别对不同用户进行权限的操作。

```bash
$ chmod o-rx,g-r file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ chmod o-rx,g-r file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwx--x--- 1 kizaiwsl2 kizaiwsl2 0 9月  16 21:56 file.txt
```

有时，我希望可以给设置的权限，而不是增加或删除权限。```chmod```命令是支持使用```=```来实现这个功能。
例如，我想把file.txt的文件设置为所有者的权限为```rwx```,组为```r--```,其他用户为```--x```。可以用下面的命令来实现。

```bash
$ chmod u=rwx,g=r,o=x file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ chmod u=rwx,g=r,o=x file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxr----x 1 kizaiwsl2 kizaiwsl2 0 9月  16 21:56 file.txt
```

以上给文件赋权限都是采用符号的模式，现在开始介绍使用数字的模式来给文件赋值权限，这也是日后最常用的赋权限的主要方式。

如果我想把file.txt的权限修改为```rwxrw-r--```,那么它对应的数字权限应该为``764```,那么可以使用以下命令来修改：
```bash
$ chmod 764 file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ chmod 764 file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxrw-r-- 1 kizaiwsl2 kizaiwsl2 0 9月  16 21:56 file.txt
```

最后，如何你想设置一个文件夹的所有文件的权限可以使用```-R```来实现。
例如，你想添加执行权限在```test```文件夹的所有文件，可以使用：
```bash
$ chmod -R +x test
```

请记住你只能修改您拥有的文件的权限，而不能修改不是您的文件权限，如果你真的需要修改别人的文件权限的话，那么可以使用```sudo```来实现.
```bash
sudo chmod <文件权限> <文件路径/文件名>
```

### 更改所有权：```chown```命令
为了更改文件或目录的所有权，您可以使用```chown```命令。它可以更改所有者和所有者的组。(还有一个```chgrp```命令，它改变了拥有文件的组，但```chown```也可以这样做。)

假设，您有一个文件file.txt,您希望将所有权更改为用户，你可以运行以下命令：
```bash
$ sudo chown root file.txt
```
不像```chmod```命令，第一次使用```chown```命令需要用到```roo```用户，后面再使用```chown```命令就不需要用```sudo```了。

可以查看执行后的结果;

```bash
kizaiwsl2@KIZAI-DESKTOP:~$ sudo chown root file.txt
[sudo] password for kizaiwsl2:
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxrw-r-- 1 root kizaiwsl2 0 9月  16 21:56 file.txt
```
假设您想将所有者组更改为```daemon```。为此，提及以类似名称需要在前面加一个```:```
```bash
$ sudo chown :daemon file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ sudo chown :daemon file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxrw-r-- 1 root daemon 0 9月  16 21:56 file.txt
```

如果您想更改所有者用户以及所有者组，把file.txt文件的用户改成```kizaiwsl2```，用户组```root```，可以运行以下命令：

```bash
$ sudo chown kizaiwsl2:root file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ sudo chown kizaiwsl2:root file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxrw-r-- 1 kizaiwsl2 root 0 9月  16 21:56 file.txt
```

有时，您可能需要将所有者更改为特定用户及其默认组。虽然您可以手动找出用户的默认组，然后运行命令，```chown``` 允许您通过排除：在末尾自动执行此操作。例如，如果您要将所有权更改为```root```及其默认组，请运行：

```bash
$ sudo chown root: file.txt

kizaiwsl2@KIZAI-DESKTOP:~$ sudo chown root: file.txt
kizaiwsl2@KIZAI-DESKTOP:~$ ls -l file.txt
-rwxrw-r-- 1 root root 0 9月  16 21:56 file.txt
```

像```chmod```命令一样,```chown```也支持```-R```标志。
例如，如果您要更改所有文件的所有者用户以root用于test目录，请运行：
```baash
$ sudo chown -R root: test
```











