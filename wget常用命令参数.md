# wget 常用命令参数
## 开始
参数|参数|功能
:-:|:-|:-
-V|--version|显示 Wget 的版本信息并退出。
-h|--help|打印此帮助。
-b|--background|启动后转入后台。
## 登入并输入文件
参数|参数|功能
:-:|:-|:-
-d|--debug|打印大量调试信息。
-q|--quiet|安静模式(无信息输出)。
-v|--verbose|详尽的输出(此为默认值)。
-nv|--no-verbose|关闭详尽输出，但不进入安静模式。
-O||下载并以不同的文件名保存(-O：下载文件到对应目录，并且修改文件名称)
## 下载
参数|参数|功能
:-:|:-|:-
-t|--tries=NUMBER|设置重试次数为 NUMBER (0 代表无限制)。
-O|--output-document=FILE|将文档写入 FILE。
-nc|--no-clobber|不要重复下载已存在的文件。
-c|--continue|继续下载部分下载的文件。
-N|-timestamping|只获取比本地文件新的文件。
-S|--server-response|打印服务器响应。
|--spider|模拟下载，不会下载，只是会检查是否网站是否好着
## 递归下载
参数|参数|功能
:-:|:-|:-
-r|--recursive|指定递归下载。
-l|--level=NUMBER|最大递归深度( inf 或 0 代表无限制，即全部下载)。
-k|--convert-links|让下载得到的 HTML 或 CSS 中的链接指向本地文件。
-K|--backup-converted|在转换文件 X 前先将它备份为 X.orig。
-m|--mirror|-N -r -l inf --no-remove-listing 的缩写形式。
-p|--page-requisites|下载所有用于显示 HTML 页面的图片之类的元素。
## 递归接受/拒绝
参数|参数|功能
:-:|:-|:-
-A|--accept=LIST|逗号分隔的可接受的扩展名列表。
-R|--reject=LIST|逗号分隔的要拒绝的扩展名列表。
-D|--domains=LIST|逗号分隔的可接受的域列表。
-H|--span-hosts|递归时转向外部主机。
-L|--relative|只跟踪有关系的链接。
-I|--include-directories=LIST|允许目录的列表。
-X|--exclude-directories=LIST|排除目录的列表。
-np|--no-parent|不追溯至父目录。
## 目录
参数|参数|功能
:-:|:-|:-
-nd|--no-directories|不创建目录。
-x|--force-directories|强制创建目录。
-nH|--no-host-directories|不要创建主目录。
-P|--directory-prefix=PREFIX|以 PREFIX/... 保存文件
