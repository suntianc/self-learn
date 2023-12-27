### etcd部署 - tar包方式

```shell
# 在当前目录下创建一个本地目录 {etcd-dir} 
mkdir -p etcd/
cd etcd/
# 下载etcd
wget https://github.com/etcd-io/etcd/releases/download/v3.3.13/etcd-v3.3.13-linux-amd64.tar.gz
# 把etcd压到一个本地目录 {etcd-dir} 
tar -zxvf etcd-v3.3.13-linux-amd64.tar.gz
# 把 {etcd-dir} 加到 PATH 环境变量。 (可选)
export PATH=$PATH:/root/etcd/etcd-v3.3.13-linux-amd64
 
# 创建一个目录 {db-dir} 用来保存 etcd 的数据文件。
mkdir -p etcd-db/
cd etcd-db/
 
# 使用最新的 v3 API (可选)
export ETCDCTL_API=3
 
# 查看etcd版本
etcd -version
```

### etcd 配置文件样例

```yaml
# etcd 配置文件

# 通用配置
# etcd节点名称
name: default
# etcd数据目录
data-dir: /var/lib/etcd
# 客户端监听地址
listen-client-urls: http://0.0.0.0:2379
# 广告客户端地址
advertise-client-urls: http://localhost:2379
# 对等节点监听地址
listen-peer-urls: http://0.0.0.0:2380
# 初始广告对等节点地址
initial-advertise-peer-urls: http://localhost:2380

# 集群配置
#initial-cluster: <初始集群配置>
#initial-cluster-state: <新建集群状态>
#initial-cluster-token: <集群令牌>
#initial-cluster-ssl: <是否开启集群间TLS加密>

# 安全配置（可选）
# 参数值根据您的安全需求进行设置
# tls:
#   cert-file: <TLS证书文件路径>
#   key-file: <TLS私钥文件路径>
#   client-cert-auth: <是否要求客户端认证>
#   trusted-ca-file: <CA证书文件路径>

# 日志配置（可选）
# 参数值根据您的日志需求进行设置
# logger: <日志配置>

# 认证配置（可选）
# 参数值根据您的认证需求进行设置
# auth:
#   token: <要求的令牌>

# 代理配置（可选）
# 参数值根据您的代理需
```

### 启动命令

```shell
# ${etcd_path} --config-file=${config_path}
etcd --config-file=/paas/iddbs/suntc/etcd/etcd-conf.yaml
```

