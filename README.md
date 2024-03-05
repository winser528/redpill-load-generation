# Syno Generation

## 如何使用?

1. 复制`sample_user_config.json`为`ds3615xs_user_config.json`或者`ds918p_user_config.json`
1. 编辑`<platform>_user_config.json`比如 918+ 就编辑 `ds918p_user_config.json` 文件
1. 添加扩展驱动：
   比如 `./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/mpt3sas/rpext-index.json`
1. 为你想要的平台和版本构建编译镜像:  
   比如 `./redpill_tool_chain.sh build ds918p-7.0-41890`
1. 为你想要的平台和版本构建引导:
   比如 `./redpill_tool_chain.sh auto ds918p-7.0-41890`

`./redpill_tool_chain.sh auto`运行结束之后，将会在宿主机的`./image`文件夹中生成 RedPill引导镜像。

`<platform>_user_config.json`文件中的`extensions`字段保持为空，会自动打包所有已安装的自定义驱动。
自定义驱动请按需添加，尽量不要加载无关驱动，否则会因为扩展驱动太大导致打包失败。

依赖: `docker`

### 查看帮助文本

`./redpill_tool_chain.sh`

```txt
Usage: ./redpill_tool_chain.sh <action> <platform version>

Actions: build, auto, run, clean, add, del, sn, pat

- build:    为指定平台版本构建工具链镜像.

- auto:     使用之前为指定平台构建的工具链映像启动工具链容器.
            更新 redpill 源并自动构建引导加载程序映像. 完成后将结束容器.

- run:      使用之前为指定平台构建的工具链映像启动工具链容器.
            交互式 Bash 终端.

- clean:    删除旧的图像和平台版本的构建缓存.
            使用“all”作为平台版本来删除图像并为所有平台版本构建缓存.

- add:      要安装扩展，您需要知道其索引文件位置，仅此而已.
            eg: add 'https://example.com/some-extension/rpext-index.json'

- del:      要删除已安装的扩展，您需要知道其 ID.
            eg: del 'example_dev.some_extension'

- sn:       为以下平台生成序列号和 MAC 地址
            DS3615xs DS3617xs DS916+ DS918+ DS920+ DS3622xs+ FS6400 DVA3219 DVA3221 DS1621+
            eg: sn ds920p

- pat:      用于解码 PAT 文件.

Available platform versions:
---------------------
ds918p-6.2.4-25556
ds918p-7.0.1-42218
ds918p-7.1.0-42661
ds920p-7.0.1-42218
ds920p-7.1.0-42661
ds923p-7.1.1-42962
ds1621p-7.0.1-42218
ds1621p-7.1.0-42661
ds2422p-7.0.1-42218
ds3615xs-6.2.4-25556
ds3615xs-7.0.1-42218
ds3615xs-7.1.0-42661
ds3617xs-7.0.1-42218
ds3617xs-7.1.0-42661
ds3622xsp-7.0.1-42218
ds3622xsp-7.1.0-42661
dva3221-7.0.1-42218
dva3221-7.1.0-42661

Custom Extensions:
---------------------
jumkey.acpid2
thethorgroup.boot-wait
thethorgroup.virtio

Check global_settings.json for settings.
```

## 更多细节

编译`DS920+`和`DS1621+`需要加入`jumkey.dtb`扩展并参考[这里](https://github.com/jumkey/redpill-load/blob/develop/redpill-dtb/README.md)创建设备的二进制设备树

查看基于[test.yml](https://github.com/tossp/redpill-tool-chain/blob/master/.github/workflows/test.yml)的使用[示例](https://github.com/tossp/redpill-tool-chain/actions/workflows/test.yml)
