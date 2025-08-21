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


---
### magento版本
1. 安装 xhprof 扩展

```shell
sudo pecl install xhprof
## 以及需要的依赖库
sudo apt install graphviz
```
2. 配置 php.ini

```xml
[xhprof]
extension=xhprof.so;
### 面的路径代表生成记录的位置
xhprof.output_dir=/var/www/html/localhost.shinetechmagento.com/output
```

3. 在本地新建一个域名 localhost.shinetechmagento.com 可访问的，并放入如下目录

[XHPROF](https://github.com/lzyenjoy/xhprof)

4. 根据不同的php版本下载不同的xhprof.tagz 文件 (https://pecl.php.net/package/xhprof) ，此链接是php8版本的

[xhprof 2.3.9](https://pecl.php.net/get/xhprof-2.3.9.tgz)

5. 下载后解压复制里面的 xhprof_html 和 xhprof_lib 文件夹以覆盖上述第3步下载的文件夹 public_html、xhprof_lib

6. 将下列文件放置在 magento的pub/index.php中，原来的index.php跟改为index.php.bak

```php

if (isset($_SERVER["HTTP_XHPROF"])) {

    xhprof_enable(XHPROF_FLAGS_CPU+XHPROF_FLAGS_MEMORY);

    include 'index.php.bak';

    $data = xhprof_disable();

    // Here need defind XHPROF_ROOT
    $XHPROF_ROOT = '/var/www/html/localhost.shinetechmagento.com'; // 容器里面的目录（如果是docker）
    $XHPROF_DOMAIN = 'https://localhost.shinetechmagento.com/public_html'; //可访问的域名
    require_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_lib.php";
    require_once $XHPROF_ROOT . "/xhprof_lib/utils/xhprof_runs.php";

    $tag = $_SERVER['REQUEST_URI'];
    $replace = ['/', '?', '&', '.'];
    foreach ($replace as $str) {
        $tag = str_replace($str, '-', $tag);
    }
    $tag = date('Y-m-d_H_i_s', time()) . '----' . $tag;

    $objXhprofRun = new XHProfRuns_Default();
    $run_id = $objXhprofRun->save_run($data, $tag);
    $url = $XHPROF_DOMAIN . '/index.php?source=' .$tag  . '&run=' . $run_id;
    if (isset($_SERVER["XHPROF_JUMP"])) {
        echo '<a target="_blank" href="' . $url . '">' . $run_id . '</a>';
    }
} else {
    include 'index.php.bak';
}
```

7. 直接访问 https://localhost.shinetechmagento.com/public_html 就能够拿到信息
