# core
key points of torch file system.

## type

- BLOCK
- OBJECT

## device

- SSD
- HDD

## etc

- /etc/torchfs/
  - c/settings.json
  - d/settings.json
  - sshkeys.json

## deploy

- manager:updated

    负责设置变更、数据同步。

- database:metad

    负责设置、数据属性具体落实。

- gateway:transferd

    负责对接数据上传下载。

- repository:versiond

    负责数据持久化及版本管理。

### all-in-one

### distributed

## flow

从用户的读写请求角度，串讲整个过程。

- 读

    比如要读取某个文件，会通过GET方法从相应的storages命名空间下载。

    request --> gateway --> database --> repository --> reponse

- 写

    比如要写入某个文件，会通过PUT方法到相应的storages命名空间上传。

    此时涉及到写入已存在的文件、写入不存在的文件两种情况。

    request --> gateway --> database ||-> repository --> database ||-> repository --> response

    并且写入完成之前，须要同步各个副本。

    ||-> manager ||-> response

## technology

其中：

    由database自身分布式保证metad无单点故障及强一致性。

    通过富客户端对数据做初步分类，配合定制gateway最终完成数据存储方式的选择。

    数据版本交给git处理。

特色在于任意节点均保有逻辑整体、而没有算术整体，提升物理泄露防护。
