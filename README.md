# CPIAC
Per seguente progetto sono state valutate le somiglianze e le differenze nell'utilizzo di plugin CNI per una tologia di rete che utilizza Kubernetes e Containerlab.

La topologia di rete utilizza:
- un cluster Kubernetes contentente un control-plane e due nodi worker (ciascuno dotato di rispettivi pods)
- un host esterno al cluster
- un router che collega i componenti

### CNI Plugin
Per la comunicazione tra le due reti sono stati usati dei plugin CNI: Calico e Cilium. Valutando similitudini e differenze.

### How to start
Per studiare la topologia è necessario utilizzare:
- docker
- kubernetes
- containerlab
- git

**Nota**: per l'installazione dei plugin CNI si vedano le sezioni successive.

La topologia può essere avviata con:
```
git clone https://github.com/martinaborgstrom/CPIAC
sudo containerlab -t topology/topo.yaml deploy
```

Avvio dei pods:
```
kubectl apply -f pods/server.yaml
kubectl apply -f pods/client.yaml
kubectl apply -f pods/client2.yaml
kubectl apply -f pods/iperf.yaml
```

### Calico
Per l'installazione di Calico:
```
kubectl create -f calico/tigera-operator.yaml
kubectl create -f calico/custom-resources.yaml
```

### Cilium
Per l'installazione di Cilium:
```
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
CLI_ARCH=amd64
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
```

Credits:
Per tale progetto, si è partiti dal seguente studio: ```https://virtualizestuff.com/exploring-containerlab```