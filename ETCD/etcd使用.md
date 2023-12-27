## 1. etcd租约 -V3版本

> 设置一个对象，挂接多个key共享一个过期实体，这个实体就被称为租约（lease）。租约过期后，挂接在上面的所有key都会被删除。这样一来，把相同过期时间的key挂接到同一租约上，不同过期时间的key挂接到不同租约上，就把对key的过期管理转变为了对租约的管理

### 1.1 Linux命令行

```shell
etcdctl lease grant 60 #创建一个持续时间为60秒的租约，并返回租约ID。
etcdctl lease keep-alive 694d7d17eaab280f #续约，刷新指定租约的时间，重新计时。
etcdctl lease list #枚举所有的租约，并显示租约的ID和过期时间。
etcdctl lease revoke 694d7d17eaab280f #销毁指定的租约。
etcdctl lease timetolive 694d7d17eaab280f #获取指定租约的信息，包括剩余时间和过期时间
etcdctl put hello world --lease=694d7d17eaab280f # 挂载租约
```

### 1.2 Java客户端

**pom.xml**

```xml
<dependency>
    <groupId>io.etcd.jetcd</groupId>
    <artifactId>jetcd-core</artifactId>
    <version>0.5.0</version>
</dependency>
```

**code 示例**

```java
import io.etcd.jetcd.*;
import io.etcd.jetcd.lease.LeaseGrantResponse;
import io.etcd.jetcd.options.LeaseOption;

import java.util.concurrent.ExecutionException;

public class CreateLeaseExample {
    private static final String ETCD_ENDPOINT = "http://localhost:2379";

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建etcd客户端
        Client client = Client.builder().endpoints(ETCD_ENDPOINT).build();

        // 创建租约
        Lease leaseClient = client.getLeaseClient();
        LeaseGrantResponse leaseGrantResponse = leaseClient.grant(10).get();
        long leaseId = leaseGrantResponse.getID();
        System.out.println("Lease created with ID: " + leaseId);

        // 使用租约
        KV kvClient = client.getKVClient();
        kvClient.put(ByteSequence.fromString("key"), ByteSequence.fromString("value"), PutOption.newBuilder().withLeaseId(leaseId).build()).get();
        System.out.println("Key-value pair created with lease ID: " + leaseId);

        // 关闭客户端
        client.close();
    }
}

```

## 2. etcd 基本操作 - http

详见 -- [etcd-API-en](etcd-API-en.md)