### 初始化集群基础信息

* 主机名
```bash
ansible "k8s-m1" -m shell -a "hostnamectl set-hostname k8s-m1" -i k8s-hosts.ini
ansible "k8s-m2" -m shell -a "hostnamectl set-hostname k8s-m2" -i k8s-hosts.ini
ansible "k8s-m3" -m shell -a "hostnamectl set-hostname k8s-m3" -i k8s-hosts.ini
```
* IP hosts & Disable swap for kubectl
```bash
ansible-playbook update_hosts.yml -i k8s-hosts.ini
```

### Install Docker
```bash
ansible-playbook -i k8s-hosts.ini 02-install_docker.yml
```

### Install kubelet kubeadm
```bash
ansible-playbook -i k8s-hosts.ini 03-install_kubelet.yml
```

### Setup & install haproxy

* download haproxy roles
```bash
ansible-galaxy install geerlingguy.haproxy -p .
```

* setup hostname
```bash
ansible "haproxy*" -m shell -a "hostnamectl set-hostname haproxy1" -i k8s-hosts.ini
```

* install haproxy to target hosts
```bash
ansible-playbook -i k8s-hosts.ini 04-install_haproxy.yml
```

### Use kubeadm init cluster master control plan

* Init first master
```bash
ansible-playbook 05-config-master.yml -i k8s-hosts.ini
```
