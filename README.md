# Kubernetes CoreOS

## Install Linux binaries

```
sudo mkdir -p /opt/kubernetes/bin
cd /opt/kubernetes/bin
wget https://github.com/kelseyhightower/kubernetes-coreos/releases/download/v0.0.1/kubernetes-coreos.tar.gz | tar -x -
sudo tar -xvf kubernetes-coreos.tar.gz
sudo chmod +x apiserver  controller-manager  kubecfg  kubelet proxy
sudo rm kubernetes-coreos.tar.gz
```

### Add the systemd units

```
cd $HOME
git clone https://github.com/kelseyhightower/kubernetes-coreos.git
sudo cp kubernetes-coreos/units/* /etc/systemd/system/
```

### Start the Kubernetes services

```
sudo systemctl start kubernetes-apiserver
sudo systemctl start kubernetes-controller-manager
sudo systemctl start kubernetes-kubelet
sudo systemctl start kubernetes-proxy
```

### Run the pod

```
/opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 -c kubernetes-coreos/pods/redis.json create /pods
/opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 list /pods
```

#### Test the redis server

```
docker run -t -i dockerfile/redis /usr/local/bin/redis-cli -h 172.17.42.1
```

> 172.17.42.1 is the docker gateway

### Delete the pod

```
/opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 delete /pods/redis
```
