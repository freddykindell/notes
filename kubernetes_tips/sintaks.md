**Menghapus pods dengan status Terminating**
dimana `-n` adalah namespace
```
kubectl -n namespace get pod | grep Terminating | awk '{print $1}' | xargs kubectl -n namespace delete pod -o name --grace-period=0 --force
```

**Restart semua pods**
dimana `-n` adalah namespace
```
kubectl -n namespace rollout restart deploy deploymentname
```

**Labeling worker**
```
kubectl label nodes kworker4 node=worker-04 --overwrite
```

**Scalling deployment**
dimana `-n` adalah namespace
```
kubectl scale -n namespace  deployment/nginxsv-2 --replicas=2
```

**Konfigurasi Args**
```
vi /etc/sysconfig/kubelet
```
Tambahkan argumen berikut, `--cgroup-driver=cgroupfs`
modifikasi dari `KUBELET_EXTRA_ARGS=`
menjadi `KUBELET_EXTRA_ARGS=--cgroup-driver=cgroupfs`

**Verifying the Cluster**
```
kubectl cluster-info
kubectl get nodes
kubectl get cs
```