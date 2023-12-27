### K8s 故障排查常用命令

- **kubectl get - 显示资源列表**

  ```shell
  # kubectl get 资源类型
  
  #获取类型为Deployment的资源列表
  kubectl get deployments
  
  #获取类型为Pod的资源列表
  kubectl get pods
  
  #获取类型为Node的资源列表
  kubectl get nodes
  ```

  > 名称空间
  >
  > 在命令后增加 `-A` 或 `--all-namespaces` 可查看所有名称空间中的对象，使用参数 `-n` 可查看指定名称空间的对象，例如
  >
  > ```shell
  > # 查看所有名称空间的 Deployment
  > kubectl get deployments -A
  > kubectl get deployments --all-namespaces
  > 
  > # 查看 kube-system 名称空间的 Deployment
  > kubectl get deployments -n kube-system 
  > ```
  >
  > > 并非所有对象都在名称空间里

- **kubectl describe - 显示有关资源的详细信息**

  ```shell
  # kubectl describe 资源类型 资源名称
  
  #查看名称为nginx-XXXXXX的Pod的信息
  kubectl describe pod nginx-XXXXXX	
  
  #查看名称为nginx的Deployment的信息
  kubectl describe deployment nginx		
  ```

- **kubectl logs - 查看pod中的容器的打印日志（和命令docker logs 类似）**

  ```shell
  # kubectl logs Pod名称
  
  #查看名称为nginx-pod-XXXXXXX的Pod内的容器打印的日志
  #本案例中的 nginx-pod 没有输出日志，所以您看到的结果是空的
  kubectl logs -f nginx-pod-XXXXXXX
  ```

- **kubectl exec - 在pod中的容器环境内执行命令(和命令docker exec 类似)** 

  ```shell
  # kubectl exec Pod名称 操作命令
  
  # 在名称为nginx-pod-xxxxxx的Pod中运行bash
  kubectl exec -it nginx-pod-xxxxxx /bin/bash
  ```

- **kubectl replace --force -f  xxx.yaml 指定yaml方式重启 pod 刷新部署**

- **kubectl get pod {podname} -n {namespace} -o yaml | kubectl replace --force -f -  使用 "-o yaml"参数导出Pod模板并重建模板**