图图图图图图 P28图3-1


查找应用在本地存储的所有数据，最快的方法是查看~/Library/Developer/CoreSimulator/Devices目录。

##1. plist文件

plist文件，即属性列表文件，用于存储应用程序的配置数据，这些信息都是CoreFoundation数据类型，包含了很多嵌套的键值对。
##2. 设备目录
寻找涉笔信息的方法：
* 方法一，去Devices目录下查看.default_created.plist文件
* 方法二，使用grep命令找出所有的device.plist文件

##3. Bundle目录
* Bundle目录中有一个Applications目录，该目录包含设备上所有的应用目录，这些应用目录用应用程序的bundle ID命名。在应用目录中，.app文件夹存储应用程序的核心二进制代码、图像资源、本地化信息等；Info.plist文件包含应用程序的核心配置信息，包含bundle标识、主程序包、应用程序的UI信息以及应用程序需要向设备请求的功能。
* 在文件系统中，这些plist会以XML或二进制格式进行存储，后者通常是默认的存储方式。可以通过代码的方式获取Info.plist中的信息，一般用这种方式来载入一些样式或本地化信息。

##4. Data目录
###4.1 Applications目录
Data目录中最重要的是Applications子目录。Data/Applications目录包含了应用程序运行所需要的其他数据：参数设置、缓存、cookie等。大部分数据泄露都发生在这个子目录下。
###4.2 Documents和Inbox目录
Documents目录主要用来存储非临时状态的应用数据，如用户创建的内容或应用程序在离线模式下需要的本地缓存信息。

其他应用发送过来的文件存储在Documents/Inbox目录中。你只能读取或删除存储在Inbox目录下的文件，这些来自于其他应用程序的文件不能写入你的应用目录里，它们将被一个优先级更高的系统进程处理。
###4.3 Library目录
Library目录包含应用程序相关的大部分文件，包括由应用程序或网络产生的缓存数据。该目录下的文件可以通过iTunes和iCloud进行备份，caches目录除外。
###4.4 Application Support目录
该目录主要用来存储应用程序使用的数据文件，即该目录下的文件主要用来支持应用程序运行。
###4.5 Caches和Snapshots目录
当应用程序进入后台，操作系统会自动把当前应用的屏幕快照存储到Caches/Snapshots目录中，这样做会无形中把一些敏感信息存储到本地。
Caches目录有事也会把网页缓存内容存储在子目录Caches/com.mycompane.myapp中，该位置容易泄露敏感信息，因为通过HTTPS进行长时间传输的数据可以被iOS缓存。
###4.6 Cookies目录
iOS上的cookies不会再应用之间共享，每个应用都拥有单独的目录来存储自己的cookie
###4.7 Preferences目录
iOS将应用的偏好位置存储在Preferences目录下，但是不允许应用直接编辑目录中的文件。
审查一个应用的本地存储信息时，一定要记得检查Preferences目录中的plist文件，有时候可能在这些plist文件中发现用户名和密码、API访问密钥或不应该暴露给用户的安全设置。
###4.8 保存应用程序状态的目录
为了让用户在应用间切换时使应用的界面与先前的保持一致，将对象状态信息存储在Saved Application State目录中，该处是一个容易泄露数据的地方。
###4.9 tmp目录
tmp与Caches类似，但是tmp严格存储着由应用产生的临时数据，如果这些文件在重新访问前被删除，并不能重新获取到它们。
而Caches缓存的文件可能会被再次获取或重新创建。
###4.10 Shared目录
该目录用来为应用程序提供一个共享的应用组。
当在检查存储在preference中的敏感信息或其他隐私数据时，要记得检查这个目录。