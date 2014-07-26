# Arcus
NAVER에서 오픈소스로 발표한 [Arcus]를 [Docker]로 구성 할수 있도록 Dockerfile을 만들어 보았습니다.

Step 1,2,3,4 이후에는 이 [Link]를 통해 "4. Arcus 설정"부터 따라 하시면 됩니다.

### Step 1. Git clone
```
root@ruo91:~# git clone git://github.com/ruo91/docker-arcus /opt/docker-arcus
```

### Step 2. Container Image
HostOS에서 Arcus Admin과 Memcached로 사용될 Container의 이미지를 만듭니다.

- Arcus Admin
```
root@ruo91:~# docker build --rm -t arcus-admin /opt/docker-arcus/arcus
```

- Memcached
```
root@ruo91:~# docker build --rm -t arcus-memcached /opt/docker-arcus/arcus-memcached
```

### Step 3. Container 실행
Arcus Admin으로 사용될 Container를 하나 생성하고, Memcached로 사용될 Container를 3개 생성합니다.

- Arcus Admin
```
root@ruo91:~# docker run -d --name="arcus-admin" -h "arcus" arcus-admin
```

- Memcached
```
root@ruo91:~# docker run -d --name="arcus-memcached-1" -h "memcached-1" arcus-memcached
root@ruo91:~# docker run -d --name="arcus-memcached-2" -h "memcached-2" arcus-memcached
root@ruo91:~# docker run -d --name="arcus-memcached-3" -h "memcached-3" arcus-memcached
```

### Step 4. Container IP 확인 및 SSH 접속
- Arcus Admin, Memcached 1,2,3
arcus-admin의 비밀번호은 arcus이고, arcus-memcached의 비밀번호는 memcached 입니다.
```
root@ruo91:~# docker inspect -f '{{ .NetworkSettings.IPAddress }}' \
arcus-admin arcus-memcached-1 arcus-memcached-2 arcus-memcached-3
```
```
root@ruo91:~# ssh `docker inspect -f '{{ .NetworkSettings.IPAddress }}' arcus-admin`
root@ruo91:~# ssh `docker inspect -f '{{ .NetworkSettings.IPAddress }}' arcus-memcached-1`
root@ruo91:~# ssh `docker inspect -f '{{ .NetworkSettings.IPAddress }}' arcus-memcached-2`
root@ruo91:~# ssh `docker inspect -f '{{ .NetworkSettings.IPAddress }}' arcus-memcached-3`
```

Thanks :)
[Arcus]:http://naver.github.io/arcus
[Docker]:https://docker.com/
[Link]:http://www.yongbok.net/blog/arcus-open-source-cloud-cache-server/
