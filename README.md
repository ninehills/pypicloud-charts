# PyPi Cloud Helm Chart

This chart installs a PyPI server: <https://github.com/stevearc/pypicloud>

Ref: <https://github.com/owkin/charts/tree/master/pypiserver>

## Prerequisites Details

- Kubernetes 1.6+
- PV dynamic provisioning support on the underlying infrastructure

## Todo

- Put RSA Key in `Secret`

## 配置指南

### 存储后端

支持的存储后端包括两类

- 对象存储，AWS S3 / GCE GCS / Azure Blob Storage
    - 通过配置项开启 <https://pypicloud.readthedocs.io/en/latest/topics/storage.html#storage>
- 持久卷，在`values.yaml`中开启`persistence: true`后，选择`storageClass`即可

高可用部署，需要存储后端为对象存储或者分布式持久卷。

### 访问入口

支持 Service 和 Ingress 两种，分别在`values.yaml`中配置，默认使用NodePort

### 管理权限

默认管理员账号`admin`，密码`admin`，可以在`values.yaml`中修改

### 高可用

1. 存储后端高可用：使用对象存储或者分布式持久卷作为存储后端，注意使用分布式持久卷时，需要将`accessMode`设置为`ReadWriteMany`
2. 缓存高可用：默认将包的MetaData缓存到本地sqlite数据库中，高可用场景可以使用MySQL/PosgreSQL/Redis作为缓存，但是使用本地缓存也可以提供多实例服务。

### 其他行为

1. 包不存在时，默认为重定向到上游，如果没有上游，请配置 `pypi.fallback = none`
