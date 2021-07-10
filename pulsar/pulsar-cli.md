# Pulsar命令行工具

### stanalone 启动
bin/pulsar standalone ： 当前terminal运行，terminal关闭，服务关闭
pulsar-daemon start/stop standalone ： 后台运行的standalone服务模式
client

## 生产

bin/pulsar-client produce my-topic --messages "hello-pulsar"

向 my-topic 这个topic生产数据，内容为“hello-pulsar”，如果topic不存在，pulsar会自动创建

## 消费

bin/pulsar-client consume my-topic -s "first-subscription"

消费my-topic的数据，订阅名称为“first-subscription", 如果topic不存在，pulsar会自动创建

​

## tenants
查看所有tenants

/pulsar-admin tenants list

### 创建tenants

pulsar-admin tenants create my-tenant

### 删除tenants

pulsar-admin tenants delete my-tenant

## broker
### 查看存活的broker信息

pulsar-admin brokers list use

### 查看broke如上的namesapce

pulsar-admin brokers namespaces use --url broker1.use.org.com:8080

### 查看可以动态更新的配置

pulsar-admin brokers list-dynamic-config

### 查看已经动态更新过的配置

pulsar-admin brokers get-all-dynamic-config

## 动态更新配置

示例：

pulsar-admin brokers update-dynamic-config brokerShutdownTimeoutMs 100

## namespace
### 查看tenant下的所有namespace

pulsar-admin namespaces list test-tenant

### 创建namespace

pulsar-admin namespaces create test-tenant/test-namespace

### 查看namespace策略

pulsar-admin namespaces policies test-tenant/test-namespace

### 删除namespace

pulsar-admin namespaces delete test-tenant/ns1

### permission
pulsar的权限控制是在namespace级别的，

## 授权

pulsar-admin namespaces grant-permission test-tenant/ns1 \

--actions produce,consume \

--role admin10

### 注意： 当broker.conf中的authorizationAllowWildcardsMatching 为true时，支持通配符匹配，例如，

pulsar-admin namespaces grant-permission test-tenant/ns1 \

--actions produce,consume \

--role 'my.role.*'

### 获取授权信息

pulsar-admin namespaces permissions test-tenant/ns1

## 撤销授权

pulsar-admin namespaces revoke-permission test-tenant/ns1 \

--role admin10

persistent topics
## 格式： persistent://tenant/namespace/topic

## 查看namespace下的topic信息

pulsar-admin persistent list my-tenant/my-namespace

### 列举persistent topic

pulsar-admin topics list tenant/namespace

### 给客户端添加针对于某个topic的role（许可）

pulsar-admin persistent grant-permission

--actions produce,consume --role application1

persistent://test-tenant/ns1/topic1

### 获取许可信息

pulsar-admin persistent permissions \

persistent://test-tenant/ns1/tp1

## 回滚许可

pulsar-admin persistent revoke-permission \

--role application1 \

persistent://test-tenant/ns1/tp1 \

### 删除topic

pulsar-admin persistent delete \

persistent://test-tenant/ns1/tp1 \

### 下线topic

pulsar-admin persistent unload \

persistent://test-tenant/ns1/tp1

### 查看topic相关的统计信息

pulsar-admin persistent stats \

persistent://test-tenant/ns1/tp1

### 查看topic内部统计信息

pulsar-admin persistent stats-internal \

persistent://test-tenant/ns1/tp1

### peek 消息

pulsar-admin persistent peek-messages \

--count 10 --subscription my-subscription \

persistent://test-tenant/ns1/tp1

### 跳过消费部分消息

pulsar-admin persistent skip \

--count 10 --subscription my-subscription \

persistent://test-tenant/ns1/tp1

跳过所有数据

pulsar-admin persistent skip-all \

--subscription my-subscription \

persistent://test-tenant/ns1/tp1 \

重置消费cursor到几分钟之前

pulsar-admin persistent reset-cursor \

--subscription my-subscription --time 10 \

persistent://test-tenant/ns1/tp1 \

查找topic所在的broker信息

pulsar-admin persistent lookup \

persistent://test-tenant/ns1/tp1 \

获取topic的bundle信息

pulsar-admin persistent bundle-range \

persistent://test-tenant/ns1/tp1 \

"0x00000000_0xffffffff"

查询topic的订阅信息

pulsar-admin persistent subscriptions \

persistent://test-tenant/ns1/tp1 \

取消订阅

pulsar-admin persistent unsubscribe \

--subscription my-subscription \

persistent://test-tenant/ns1/tp1 \

最后一条消息的MessageID

pulsar-admin topics last-message-id topic-name

non-persistent topics
格式 ： non-persistent://tenant/namespace/topic

获取统计信息

pulsar-admin non-persistent stats \

non-persistent://test-tenant/ns1/tp1 \

获取内存统计信息

pulsar-admin non-persistent stats-internal \

non-persistent://test-tenant/ns1/tp1 \

创建分区topic

bin/pulsar-admin non-persistent create-partitioned-topic \

non-persistent://my-tenant/my-namespace/my-topic \

--partitions 4

注意：需要指明topic名称和分区数量

分区topic的元数据信息

pulsar-admin non-persistent get-partitioned-topic-metadata \

non-persistent://my-tenant/my-namespace/my-topic

下线topic

pulsar-admin non-persistent unload \

non-persistent://test-tenant/ns1/tp1

分区topic
格式： persistent://tenant/namespace/topic

创建topic

bin/pulsar-admin topics create-partitioned-topic \

persistent://my-tenant/my-namespace/my-topic \

--partitions 4

创建非分区topic

$ bin/pulsar-admin topics create persistent://my-tenant/my-namespace/my-topic

获取分区topic的元数据信息

pulsar-admin topics get-partitioned-topic-metadata \

persistent://my-tenant/my-namespace/my-topic

更新topic信息

pulsar-admin topics update-partitioned-topic \

persistent://my-tenant/my-namespace/my-topic \

--partitions 8

注意：修改分区数量时，只能比原来的分区数大

删除topic

bin/pulsar-admin topics delete-partitioned-topic \

persistent://my-tenant/my-namespace/my-topic

获取统计信息

pulsar-admin topics partitioned-stats \

persistent://test-tenant/namespace/topic \

--per-partition

获取内部统计信息

pulsar-admin topics stats-internal \

persistent://test-tenant/namespace/topic

Schema
上传schema

pulsar-admin schemas upload <topic-name> --filename /path/to/schema-definition-file

获取schema

pulsar-admin schemas get <topic-name>

删除schema

pulsar-admin schemas delete <topic-name>



