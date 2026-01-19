# KUBERNETES – ANOTAÇÕES DO CURSO

## Conceitos Gerais

**Orquestração**
Gerenciar diversos servidores da melhor forma possível. Se um servidor falhar, outro assume automaticamente.

**Objetivos do Kubernetes**

* Alta disponibilidade
* Escalabilidade
* Redução de custos de infraestrutura
* Automação de deploys

**Balanceamento de carga (Load Balance)**
Várias máquinas entregando o mesmo projeto, dividindo o tráfego.

**Execução distribuída**
O Kubernetes permite rodar o mesmo projeto em várias máquinas sem precisar configurar manualmente cada uma.

**Origem**
Criado pelo Google e open-source desde 2014. Roda em qualquer ambiente (cloud, VPS ou on-premises).

---

## Arquitetura do Kubernetes

### Control Plane

* Máquina responsável por gerenciar o cluster
* Decide onde os Pods rodam
* Gerencia atualizações, escalabilidade e estado desejado
* Ao atualizar uma imagem no Control Plane, os Nodes replicam a mudança

### Nodes

* Servidores de trabalho (VPS ou máquinas físicas)
* Onde os Pods e containers realmente rodam

---

## Principais Recursos

### Deployment

* Gerencia a aplicação
* Define:

  * Imagem do container
  * Número de réplicas
  * Estratégia de atualização
* Não executa diretamente a aplicação, apenas garante o estado desejado

### Pod

* Menor unidade do Kubernetes
* Contém um ou mais containers
* Ambiente isolado (IP, portas e volumes próprios)
* Normalmente possui apenas um container

### Service

* Responsável por expor os Pods
* Cria um ponto de acesso estável para a aplicação
* Atua como uma entidade de rede independente

---

## Ferramentas

### kubectl

* Interface de linha de comando do Kubernetes
* Equivalente ao `docker run`, porém para o cluster
* Usado para criar, inspecionar e gerenciar recursos

---

## Minikube

Ferramenta para rodar Kubernetes localmente.

**Iniciar o Minikube**

```bash
minikube start --driver=docker
```

**Parar o Minikube**

```bash
minikube stop
```

**Acessar o Dashboard**

```bash
minikube dashboard
minikube dashboard --url
```

---

## Deployments (via terminal)

**Criar um Deployment**

```bash
kubectl create deployment nome-deployment --image=ryanmosc/flask-kub-projeto
```

**Deletar Deployment**

```bash
kubectl delete deployment nome-deployment
```

**Listar Deployments**

```bash
kubectl get deployments
```

**Detalhar um Deployment**

```bash
kubectl describe deployment nome-deployment
```

**Ver configurações do cluster**

```bash
kubectl config view
```

---

## Services

**Listar Deployments (para obter o nome)**

```bash
kubectl get deployments
```

**Criar um Service para um Deployment**

```bash
kubectl expose deployment nome-deployment --type=LoadBalancer --port=5000
```

**Deletar um Service para um Deployment**

```bash
kubectl delete service nome-service
```

**Acessar o Service no Minikube (ambiente de teste)**

```bash
minikube service nome-deployment
```

**Listar Services**

```bash
kubectl get services
```

**Detalhar um Service específico**

```bash
kubectl describe service nome-deployment
```

---

## Escalabilidade

**Escalar número de réplicas (Pods)**

```bash
kubectl scale deployment/nome-deployment --replicas=5
```

* Cria múltiplos Pods para redundância
* Garante uptime e alta disponibilidade

**Reduzir ou aumentar réplicas**

```bash
kubectl scale deployment/nome-deployment --replicas=3
```

**Ver ReplicaSets**

```bash
kubectl get rs
```

---

## Atualização de Imagens

**Atualizar imagem do Deployment**

* Sempre rebuildar a imagem
* Utilizar uma nova tag

```bash
kubectl set image deployment/nome-deployment flask-kub-projeto=ryanmosc/flask-kub-projeto:2
```

---

## Rollback

**Voltar para a versão anterior da aplicação**

```bash
kubectl rollout undo deployment/nome-deployment
```

---

## Observação Importante

* Em produção, os recursos normalmente são definidos em arquivos YAML
* O uso do terminal é mais comum para testes, aprendizado e troubleshooting
