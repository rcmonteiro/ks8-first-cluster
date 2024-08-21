# Criando um primeiro cluster K8S

```bash
# Criando um cluster K8S
kind create cluster --name first-cluster

# Verificando o cluster criado
kubectl cluster-info --context kind-first-cluster

# Verificando os nodes do cluster
kubectl get nodes

# Verificando os pods do cluster, que não tem nada pois não criamos nenhum
kubectl get pods
```

Podemos usar o lens para visualizar os pods e os nodes de maneira gráfica, ou usar o `kubectl` 

Não é o ideal ter o control plane junto com outros pods no mesmo node, vamos separar isso.

```bash

# Deletar o cluster (junto com o node)
kind delete cluster --name first-cluster

# Criando um novo cluster baseado no manifesto criado `kind.yaml`
kind create cluster --name first-cluster --config kind.yaml

# Verificando o cluster criado
kubectl cluster-info --context kind-first-cluster
```
