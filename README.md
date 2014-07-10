github.com/GoogleCloudPlatform/kubernetes

## Compile the binaries

```
export GOOS=linux
bash hack/build-go.sh 
+++ Building proxy
+++ Building integration
+++ Building apiserver
+++ Building controller-manager
+++ Building kubelet
+++ Building kubecfg
```

### Copy the binaries

```
cd output/go/
scp apiserver controller-manager kubelet proxy kubecfg core@coreos.example.com:~/
```

```
ssh core@coreos.example.com
chmod +x apiserver kubelet controller-manager proxy
sudo mkdir -p /opt/kubernetes/bin
sudo mv kubecfg apiserver controller-manager kubelet proxy /opt/kubernetes/bin/
```

### Add the systemd units

### Start the Kubernetes services

```
sudo systemctl start kubernetes-apiserver
sudo systemctl start kubernetes-controller-manager
sudo systemctl start kubernetes-kubelet
sudo systemctl start kubernetes-proxy
```

### Run the pod

```
/opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 -c pods/pod.json create /pods
/opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 list /pods
```
