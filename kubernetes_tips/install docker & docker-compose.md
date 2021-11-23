##Instal Docker dan Docker Compose

**Install Docker**
*jika ingin menghapus docker system lama*
```
yum remove -y docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
```

**install yum utiliity**
```
yum install -y yum-utils
```

**tambahkan repo docker-ce**
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**install paket docker-ce**
```
yum install docker-ce -y
```

**enable service pada startup dan jalankan**
```
systemctl enable docker --now
```

**download binary docker-compose**
> download dan simpan pada /usr/local/bin/docker-compose
```
curl -L https://github.com/docker/compose/releases/download/v2.0.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

**tambahkan flag +x agar binary bisa dieksekusi**
```
chmod +x /usr/local/bin/docker-compose
```
