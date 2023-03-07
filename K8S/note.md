### 容器内操作宿主机k8s
```python
res = os.popen("nsenter --mount=/host/proc/1/ns/mnt /bin/bash -c 'kubectl delete service {} -n ai'".format(serviceName))
```
