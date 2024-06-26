### Kubernetes installation on AWS ec2 instance with Ubuntu Linux distro.

### Docker Installation

1. Update the package index:
    ```bash
    sudo apt update
    ```

2. Install Docker:
    ```bash
    sudo apt install docker.io -y
    ```

3. Start and enable Docker:
    ```bash
    sudo systemctl start docker
    sudo systemctl enable docker
    ```

### Kubernetes Installation

1. Update the package index:
    ```bash
    sudo apt-get update
    ```

2. Install required packages:
    ```bash
    sudo apt-get install -y apt-transport-https ca-certificates curl gpg
    ```

3. Add the Kubernetes GPG key:
    ```bash
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    ```

4. Add the Kubernetes apt repository:
    ```bash
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    ```

5. Update the package index again:
    ```bash
    sudo apt-get update
    ```

6. Install Kubernetes components:
    ```bash
    sudo apt-get install -y kubelet kubeadm kubectl
    ```

7. Hold the installed packages to prevent them from being automatically updated:
    ```bash
    sudo apt-mark hold kubelet kubeadm kubectl
    ```

8. Enable and start kubelet:
    ```bash
    sudo systemctl enable --now kubelet
    ```

### Initialize Kubernetes Cluster

1. Initialize the Kubernetes master node:
    ```bash
    kubeadm init --apiserver-advertise-address=<MasterServerIP> --pod-network-cidr=192.168.0.0/16
    ```

2. Check kubelet service status:
    ```bash
    service kubelet status
    ```

### Configure kubectl for Ubuntu User

1. Create the .kube directory:
    ```bash
    mkdir -p /home/ubuntu/.kube
    ```

2. Copy the admin.conf to the .kube directory:
    ```bash
    cp /etc/kubernetes/admin.conf /home/ubuntu/.kube/config
    ```

3. Change ownership of the .kube directory:
    ```bash
    chown -R ubuntu:ubuntu /home/ubuntu/.kube
    ```

### Install Calico Network Plugin

1. Create the Calico resources:
    ```bash
    kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
    ```

2. Download the custom resources manifest:
    ```bash
    curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml -O
    ```

3. Apply the custom resources manifest:
    ```bash
    kubectl create -f custom-resources.yaml
    ```

4. Watch the Calico pods status:
    ```bash
    watch kubectl get pods -n calico-system
    ```

### Join Worker Nodes

1. Generate the join command:
    ```bash
    kubeadm token create --print-join-command
    ```

### Verify the Installation

1. Check kubectl version:
    ```bash
    kubectl version --client
    ```

2. List the nodes in the cluster:
    ```bash
    kubectl get nodes
    ```

3. Get cluster information:
    ```bash
    kubectl cluster-info
    ```

## 🌐 Sources
1. [gist.github.com - A simple Docker and Docker Compose install script for Ubuntu](https://gist.github.com/kungfu321/46256b2ad701e787dd8a94f87e662779)
2. [unix.stackexchange.com - How to install docker from apt on Ubuntu?](https://unix.stackexchange.com/questions/405504/how-to-install-docker-from-apt-on-ubuntu)
3. [gist.github.com - Install Docker CE on Ubuntu 17.10](https://gist.github.com/levsthings/0a49bfe20b25eeadd61ff0e204f50088)
4. [theknowledgeacademy.com - How to Install Docker on Linux? A Complete Guide](https://www.theknowledgeacademy.com/blog/install-docker-on-linux/)
5. [stackoverflow.com - Can't Install Docker in Ubuntu](https://stackoverflow.com/questions/58750608/cant-install-docker-in-ubuntu)
6. [gdevillele.github.io - Installation on Ubuntu - Docker](https://gdevillele.github.io/engine/installation/linux/ubuntulinux/)
7.[https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart]


