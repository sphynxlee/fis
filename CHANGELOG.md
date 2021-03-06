## 1.2.2

    升级fis-kernel至v1.8.0

* fis.compile方法支持对不存在的文件对象进行编译，不过没有缓存

## 1.2.1

    使用fis-optimizer-png-compressor作为png图片压缩插件

* 图片压缩支持pngcrush和pngquant压缩器选择，默认为pngcrush，如果要切换为pngquant，可在配置文件中设置：

    ```javascript
    fis.config.set('settings.optimizer.png-compressor.type', 'pngquant');
    ```
    
    或者：
    
    ```javascript
    fis.config.merge({
        settings : {
            optimizer : {
                'png-compressor' : {
                    type : 'pngquant' //default is pngcrush
                }
            }
        }
    });
    ```
    
    pngcrush压缩会保持原来的色彩位数，如果原图片的色彩超过256色，在ie6下背景无法透明，会出现灰色的填充色背景。pngquant会强制把各种png图片压缩为png8格式，因此对于png24的图片压缩后会出现一定的质量折损（基本都是可以接受的），对于需要兼容ie6的产品线推荐使用pngquant作为压缩器。

## 1.2.0

    升级fis-optimizer-pngcrush至v0.0.6

* 修复小于1k的png图片压缩bug

## 1.1.9

    升级fis-kernel至v1.7.9

* 支持roadmap.path配置指定文件不经过编译处理，例：

    ```javascript
    fis.config.merge({
        roadmap : {
            path : [
                {
                    reg : '**.js',      //所有的js文件
                    useCompile : false  //不要经过编译处理
                }
            ]
        }
    });
    ```

## 1.1.8

    升级fis-command-release至v0.8.6

* deploy支持 ``subOnly`` 参数，支持只发布子目录的需求，例如：

    ```javascript
    fis.config.merge({
        deploy : {
            local : {
                from : '/static',
                to : '../output'
            }
        }
    });
    ```
    
    如果执行 fis release -d ``local``，则把编译后的 ``/static`` 目录复制到 ``../output`` 中，得到 ``../output/static``，添加 ``subOnly`` 参数后：

    ```javascript
    fis.config.merge({
        deploy : {
            local : {
                from : '/static',
                to : '../output',
                subOnly : true
            }
        }
    });
    ```
    
    如果执行 fis release -d ``local``，则把编译后的/static的 ``子目录`` 复制到 ``../output`` 中，得到 ``../output/**``

## 1.1.7

    升级fis-kernel至v1.7.8
    升级fis-command-release至v0.8.5

* 允许没有pack配置仍然进入打包逻辑
* 修复打包处理阶段不能修改内容的bug
* .ico文件默认不做hash输出

## 1.1.6

    升级fis-optimizer-pngcrush至v0.0.6

* 修复pngcrush压缩压缩时图片不是png格式的bug

## 1.1.5

    升级fis-optimizer-clean-css至v0.0.8
    升级fis-optimizer-pngcrush至v0.0.4

* 升级clean-css依赖的版本至v1.0.12，并且不允许clean-css处理@import标记，由fis接管
* 升级node-pngcrush版本至v0.0.6，超强的压缩效果

## 1.1.3

    升级fis-kernel至v1.7.7

* 修复无后缀文件的处理失败的bug

## 1.1.2

    升级fis-command-server至v0.6.1

* 采用spawn的detached参数技术替代实现nohup功能

## 1.1.1

    升级fis-packager-map至v0.0.7

* 修复打包配置不支持单独的正则bug

## 1.1.0

    内置fis-optimizer-pngcrush插件

* 推出png图片自动压缩功能

## 1.0.10

    升级fis-kernel至v1.7.5

* 修复文本文件缓存处理bug

## 1.0.9

    升级fis-kernel至v1.7.4
    升级fis-command-release至v0.8.4

* 支持对图片进行pipe处理和缓存控制，为图片压缩做准备
* 修复新建文件没有添加文件监听的bug

## 1.0.8

    升级fis-kernel至v1.7.3

* 修复fis.util.clone的bug

## 1.0.7

    升级fis-kernel至v1.7.2
    升级fis-command-release至v0.8.3
    升级fis-packager-map至v0.0.6

* 自动上传限制了并发数，一次最多只开启5个并发的上传请求
* 调整了打包策略，例如：

    ```javascript
    fis.config.merge({
        pack : {
            'static/pkg/ui.js' : ['widget/**.js', 'components/**.js'],
            'static/pkg/others.js' : '**.js'
        }
    });
    ```
    
    如果在项目中，有a.js依赖了widget/b.js的话，在 ``1.0.6`` 或以前版本，会在吧 ``a.js`` 合并到 ``others.js`` 的时候，就近将 ``widget/b.js`` 也并入到 ``others.js``(很明显，widget/b.js也符合others.js包的合并约束条件) 。1.0.7以后，将不会做这样的处理，而将 ``widget/a.js`` 合入到 ``ui.js`` 中。

## 1.0.6

    升级fis-kernel至v1.7.1

* roadmap.path支持使用 ``useMap`` 指定文件是否入map.json表。用法：

    ```javascript
    fis.config.merge({
        roadmap : {
            path : [
                {
                    reg : '**.png',
                    useMap : true
                },
                {
                    reg : 'test/**',
                    useMap : false
                }
            ]
        }
    });
    ```

## 1.0.5

    升级fis-kernel至v1.7.0
    升级fis-packager-map至v0.0.5

* 用户可以使用namespaceConnector配置节点来定义命名空间连接符，默认为“:”。用法：

    ```javascript
    fis.config.merge({
        namespace : 'common',
        namespaceConnector : '/'
    });
    ```