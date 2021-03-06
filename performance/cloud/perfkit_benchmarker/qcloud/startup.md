> 开发参考[OpenStack SDK](https://developer.openstack.org/sdks/python/openstacksdk/users/index.html) 和

# 安装

腾讯云Qcloud API安装通过

```
pip install qcloudapi-sdk-python
```

> 腾讯云有类似OpenStack的[python-openstackclient](https://pypi.python.org/pypi/python-openstackclient)这样的命令行客户端么？
>
> 阿里云有[aliyuncli](https://pypi.python.org/pypi/aliyuncli)...
>
> 原来也有 [Tencent Cloud Command Line Interface (CLI)](https://cloud.tencent.com/document/product/440/6176?lang=en)，不过文档非常简略，只有英文版。

> 有关账号和访问安全凭证请参考 [腾讯云Python API SDK快速起步](../../../../iaas/tencent/api/qcloud_python_api_startup)

安装CLI

```
pip install qcloudcli
```

# 配置账号

```
qcloudcli configure
```

> 腾讯的qcloudcli配置必须指定Region，也就是只能对单个区域进行操作，如果要对不同区域操作，需要设置独立的运行环境

> 可支持的Region Id: `gz,hk,ca,sh,shjr,bj,sg`:
>
> * `gz` - 广州
> * `hk` - **香港**
> * `ca` - **多伦多**
> * `sh` - 上海
> * `shjr` - 上海金融
> * `bj` - 北京
> * `sg` - **新加坡**

# 命令行工具

[腾讯云命令行工具（CLI）](https://cloud.tencent.com/product/cli)

```
qcloudcli cvm help
```

# PerfKit BenchMarker代码结构

参考 [PerfKit Benchmarker增加新的云服务商](../perfkit_benchmarker_add_new_cloud_provider) 主要的代码结构可以从现有的代码结构中clone出来，然后修改命令。因为云服务商的系统架构大同小异，例如，可以从阿里云`providers/alicloud`中clone出`providers/qcloud`。


注意：执行QCloud测试前，首先编辑`perfkitbenchmarker/configs/default_config_constants.yaml` 将对应`QCloud`的配置部分设置好需要开设的规格和区域。规格和区域请参考手工开设的虚拟机：

```yaml
  QCloud:
    machine_type: cvm.s1.small1
    zone: ap-guangzhou-3
    image: null
```

> * [实例规格概述](https://cloud.tencent.com/document/product/213/115)
> * [可用区列表](https://cloud.tencent.com/document/product/213/9452#zone)
> * [错误码](https://cloud.tencent.com/document/api/377/4173#1.E3.80.81.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81)

# 限制

腾讯云的自动测试主要的限制有：

* 对EIP的每日申请限制：([每个腾讯云账户每个地域每天申购次数为 配额数*2 次。](https://cloud.tencent.com/document/product/213/5733#.E8.A7.84.E5.88.99.E4.B8.8E.E9.99.90.E5.88.B6))