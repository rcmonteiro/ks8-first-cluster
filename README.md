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

# Verificando os nodes do cluster
kubectl get nodes
```

# Rodando a primeira aplicação no K8S

Precisamos de uma aplicação já em container, e vamos usar o Pod para rodar a aplicação.

```bash
# Rodando de forma imperativa o Nginx
kubectl run nginx --image=nginx:alpine3.20-slim

# Consumir os logs do Nginx
kubectl logs nginx

# Remover o pod
kubectl delete pod nginx
```

Temos o pod rodando, mas sem ter um controlador para ele, e rodando ainda sem namespace.

Agora vamos fazer do modo correto, de forma declarativa, e usando um namespace.

```bash
# Criando um namespace
kubectl create namespace first-app

# Criando o pod
kubectl run nginx --image=nginx:alpine3.20-slim --namespace=first-app
```

Agora temos o pod rodando, e o namespace criado.
Vamos aplicar a configuração `pod.yaml` para o pod.

```bash
# Criando o pod
kubectl apply -f pod.yaml --namespace=first-app

# Verificando os pods
kubectl get pods --namespace=first-app
```

Agora fazendo um Port Forward para a 8080, a gente pode acessar o Nginx no browser.
O mesmo pode ser feito via CLI

```bash
kubectl port-forward nginx --namespace=first-app 8081:80
```

Podemos agora remover o pod.

```bash
# Remover o pod
kubectl delete pod nginx --namespace=first-app
```

Ainda temos alguns problemas, pois rodamos sem ter redundância, até agora rodamos um pod único, e isso não é ideal.

```bash