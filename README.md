# O que é o Kubernetes (K8S)
* Surgiu no google
* Orquestrator de Containers de alta performance
* Trabalha de forma distribuidaatravés de Cluster
* Escalável
* flexível e Confiável
* Vasta documentação
* Disponível em grande players de Cloud Computing GCP, Amazon, Azure e Digital Ocean

## Clusters / Nodes
* Cluster: Conjunto de máquinas (Nodes)
* Cada máquina possui uma quantidade de vCPU e Memorias

## POD
É a camada que o K8S usa para gerenciar os containers, podendo ter mais de um container mais, não é recomendado
* Pods: Unidade do Kubernetes que possui um ou mais containers
* O Pod representa os processos rodando no cluster

Cluster -> [Nodes -> [Pods -> [Containers] ] ]

Para rodar o K8S é recomendado no minimo 3 máquinas em produção

## Deployment
É um tipo do K8S que trabalha com o conceitode ReplicaSets, para podermos termos mais poder computacional

## Services
* feature que permite aoKubernetes a disponibilizar recursos de reds e autodiscovery
* Ex:
  * Um container em um pod pode rer acesso ao outro Pod através de um service
  * O acesso externo a qualqeur Pod também é realizado através de Services
* Normalmente se dão em 3 categorias
  * NodePort
  * ClusterIp: IP interno
  * Load Balancer: IP externo

## Comandos K8S
#### Criar um cluster com kind
```bash
kind create cluster --name=avancadev
```

##### Visualizar nos Nodes do Cluster
``` bash
kubectl get nodes
```

##### Visualizar os Pods dos Nodes
```bash
kubectl get pods
```

##### Visualizar os Services
```bash
kubectl get services
```

#### Criar um Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: microsservico-pagamento
spec:
  replicas: 1
  selector:
    matchLabels:
      app: microsservico-pagamento
  template:
    metadata:
      labels:
        app: microsservico-pagamento
    spec:
      containers:
        - name: microsservico-pagamento
          image: angolar/pagamento-ms:latest
          ports:
            - containerPort: 9091
```

##### Rodar um deployment
```bash
kubectl apply -f deployment.yml
```

##### Rodar um service
```bash
kubectl apply -f deployment.yml
```

##### Acessar um service localmete
```bash
kubectl port-forward service/microsservico-pagamento-service 9091:9091
```

##### Delete um pod
```bash
kubectl delete pods microsservico-pagamento-75f85dbc74-t6xnq
```

### Trabalhar com Environment usando ConfigMap
```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: microsservico-pagamento-config
  labels:
    name: microsservico-pagamento-config
data:
  env: |
    RABBITMQ_DEFAULT_USER=rabbitmq
    RABBITMQ_DEFAULT_PASS=rabbitmq
    RABBITMQ_DEFAULT_HOST=localhost
    RABBITMQ_DEFAULT_PORT=5672
    RABBITMQ_DEFAULT_VHOST=/
    RABBITMQ_CONSUMER_NAME=payment-ms
    RABBITMQ_CONSUMER_QUEUE_NAME=orders
    RABBITMQ_NOTIFICATION_EX=amq.direct
    RABBITMQ_NOTIFICATION_ROUTING_KEY=
    RABBITMQ_DLX=dlx
```

```bash
kubectl apply -f .k8s/configmap.yml
```

##### Acessar um Pod (container)
```bash
kubectl exec -it microsservico-pagamento-5b8d6c8b5d-kl7mp /bin/bash
```

### Trabalhar com Environment usando Secrets
É um tipo de objecto onde o dado fica opaque dentro do K8S em base64
```yml
apiVersion: v1
kind: Secret
metadata:
  name: microsservico-pagamento-secret
type: Opaque
data:
  RABBITMQ_DEFAULT_USER: cmFiYml0bXE=
  RABBITMQ_DEFAULT_PASS: cmFiYml0bXE=
```

```bash
kubectl apply -f .k8s/secrets.yml
```