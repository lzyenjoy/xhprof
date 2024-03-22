[XHPROF](https://github.com/longxinH/xhprof)

**1. pecl 安装**

```shell
sudo pecl install xhprof
```

对应自己的 `PHP` 版本查看, 安装自己 `PHP` 对应的版本

```shell
php -v
```
- xhprof PHP 7.x 用版本地址 [https://github.com/RustJason/xhprof/tree/php7](https://github.com/RustJason/xhprof/tree/php7)
- xhprof PHP 8.x 用版本地址(Pecl) [https://pecl.php.net/package/xhprof](https://pecl.php.net/package/xhprof)

下载后解压复制里面的 xhprof_html 和 xhprof_lib 文件夹以覆盖上述下载的文件夹 public_html、xhprof_lib

**2. 配置 php.ini**

```ini
[xhprof]
extension=xhprof.so;
; directory used by default implementation of the iXHProfRuns
; interface (namely, the XHProfRuns_Default class) for storing
; XHProf runs.
; 下面就是你的日志输出文件
;xhprof.output_dir=<where_you_want_to_save_xhprof_output_file>
xhprof.output_dir=/tmp/xhprof
```

**3. 如果出现错误**　`failed to execute cmd: " dot -Tpng". stderr: sh: 1: dot: not found '`，
所以需要先安装 `graphviz`

**4. 使用介绍**
 - 在`track/index.php`中定义自己**XHPROF_ROOT**的位置和`XHprof`的域名
 - 将要测试网站的**index.php**文件改名为**index.php.bak**
 - 将`track/index.php`放到要测试的网站根目录
 - 如果header中没有自定义｀HTTP_XHPROF｀和 ` HTTP_JUMP ` 这两个字段可以直接将`index.php`中的这两个参数改为`true`（可以使用chrome的ModHeader插件动态添加）
原作者： [https://github.com/luo3555/xhprof.git](https://github.com/luo3555/xhprof.git)
-  访问https://{{domain}}/public_html/ 可以查看所有的id

