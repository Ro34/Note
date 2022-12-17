```shell
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

如果使用systemctl
```shell
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```

需要挂载
```shell
-v /var/run/docker.sock:/var/run/docker.sock  -v /usr/bin/docker:/usr/bin/docker
```

启动api
```shell
docker run -itd --name middleware_api -p 8006:8006 -v /var/run/docker.sock:/var/run/docker.sock  -v /usr/bin/docker:/usr/bin/docker 0192aa9b9917 /bin/bash sh run.sh
```