#Migrasi Container Runtime
##Migrasi Container Runtime dari Docker ke Containerd secara Live

###Masuk ke mode maintenance
**Login ke server Master**
Setelah login lalu lakukan cordon dan drain pada salah satu worker
```
kubectl cordon kworker1
kubectl drain kworker1 --ignore-daemonsets
```
> script diatas bertujuan agar tidak ada pods yang dicreate pada worker sekaligus mengosongkan pods yang ada pada worker tersebut.

**Login ke server Worker**
Stop service kubelet, lalu disable and stop docker service
```
systemctl stop kubelet docker
systemctl disable docker
```

Modifikasi file config.toml pada sistem containerd
```
vi /etc/containerd/config.toml
```

Lalu, tambahkan simbol komen `#` pada line seperti berikut, 
dari ```disable_plugins =["cri"]``` menjadi ```#disable_plugins =["cri"]```

restart containerd service dan set menjadi run pada saat startup
```
systemctl restart containerd
systemctl enable containerd
```

Lanjut dengan memodifikasi kubeadm flags dan menambahakan 2 flags
Buka editor ke file kubeadm-flags.env
```
vi /var/lib/kubelet/kubeadm-flags.env
```
berikut flags yang harus ditambahkan
> --container-runtime=remote
> --container-runtime-endpoint=unix:///run/containerd/containerd.sock


Tambah 2 flags diatas menjadi seperti berikut
```
KUBELET_KUBEADM_ARGS="--network-plugin=cni --pod-infra-container-image=k8s.gcr.io/pause:3.5 --container-runtime=remote --container-runtime-endpoint=unix:///run/containerd/containerd.sock"
```

Start kembali kubelet service
```
systemctl start kubelet
```

###Keluar dari mode maintenance
**Login kembali ke server Master**
Setelah login lalu lakukan uncordon worker yang telah dimigrasi ke containerd
```
kubectl uncordon kworker1
```

> lakukan pada semua worker, setelah semua worker selesai lanjutkan ke master