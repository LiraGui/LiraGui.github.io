---
layout: post
title: "O que danado é um pod?"
date: 2024-04-05 14:30:00 -0300 
categories: [docker, kubernetes, container]
tags: [kubernetes]
author: guilherme
image: assets/images/8.jpg
---

# O que danado é um pod?

Neste post eu vou tentar destrinchar cada ponto dentro do contexto de um pod, sendo assim vamos ver um pouco de redes, politicas de rede, persistência de dados para pods, exemplos práticos e mais umas coisinhas aí. Esse primeiro post será a parte um de alguns posts sobre Kubernetes que vou soltar por aqui. Esse é a parte 1 sobre pods que eu vou postar.  

### $ O que é um pod?
Um [pod](https://kubernetes.io/docs/concepts/workloads/pods/) nada mais é do que o menor artefato possível de implementação dentro de um cluster Kubernetes. É nele que podemos subir um ou mais containers, compartilhando storage e recursos de rede.

Cada container em um pod executa no próprio [cgroup](https://en.wikipedia.org/wiki/Cgroups), mas eles compartilham alguns [namespaces](https://en.wikipedia.org/wiki/Linux_namespaces).

Uma característica interessante dos pods, é que eles foram criados para serem efêmeros, sendo assim quando um pod cai ele automaticamente é iniciado novamente.

Podemos subir um pod de duas maneiras, usando um manifesto (.yaml) ou por linha de comando, para faze-lo é preciso ter um ambiente com um cluster k8s rodando com [kind](https://kind.sigs.k8s.io/docs/user/quick-start/), [minikube](https://minikube.sigs.k8s.io/docs/start/) ou de outro modo. Tendo nosso cluster rodando e com o kubectl, kubelet, e o kubeadm instalados, podemos subir um pod somente pela linha de comando, basta executarmos o comando:

```sh
alias k=kubectl
# Como exemplo vou subir um pod com um container NGINX
k run nginx --image=nginx
```

Para saber se seu container subiu é so rodar o comando:

```sh
k get pods

# Ele vai retornar algo como
NAME        READY      STATUS       RESTARTS        AGE
nginx       1/1        Running      0               8m54s

```

Caso você queira saber mais sobre seu pod, como quais containers estão rodando nele, dados de rede, condições do seu pod, volumes e mais alguns dados, é so executar o comando:

```sh
k describe pod ${nome do pod}
```

Conseguimos subir um pod somente com o nosso **kubectl run**, agora vamos subir o mesmo pod com uma manifesto. O manifesto que nada mais é do que um arquivo yaml ou json, mas o que é geralmente mais usado é o yaml. Com o nosso manifesto e tendo as configurações necessárias conseguimos deployar qualquer objeto que a API do Kubernetes consiga criar, inclusive o pod.

No manifesto de um pod estão inclusos alguns campos e atributos essenciais para o deploy do nosso pod, como:
- Uma seção de **metadata** que descreve o Pod e seus rótulos. Rótulos esse
- Outra seção de **spec** para descrever os volumes

```yaml
apiVersion: v1
# kind será o valor do tipo de objeto que vamos criar 
kind: Pod
metadata:
  name: myapp
  labels:
    # O label é muito importante pois é por ele que outros objetos podem achar nosso pod
    name: myapp
spec:
  containers:
  - name: pod-learn
    image: nginx
    resources:
      # limits delimita o limite de CPI e memory do nosso container
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 80
```

E para subir um pod a partir de um manifesto é so executar o comando:

```sh
k apply -f ${nome do arquivo}.yaml
```


### $ Como usa-lo ?
Ao usar manifestos o Pod pode ser usado de maneiras diferentes, por objetos diferentes, como:
- Deployment
- Job
- DeamonSet

Vamos entender como funciona um pouquinho cada um desses carinhas

~> Deployment

- O objeto Deployment é uma das maneira de usar um Pod. Ao criar um deployment, o k8s cria juntamente com o pod, um `ReplicaSet`. O Deployment existe para gerenciar o lançamento de versões. Como esse post não é focado em Deployments, por isso não vou me estender sobre ele. 
- ReplicaSets  
  - Os ReplicaSets, são os responsáveis por controlar e manter a quantidade de réplicas que estabelecidas para o nosso deployment. E esses pods são identificados pela metadata pertencente ao deployment.  
  - A quantidade de réplicas são ditas na hora da criação do deployment.  

Aqui está um exemplo de como criar um Deployment
```yaml
  # Exemplo coletado no https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    labels:
      app: nginx
  spec:
    # Replicas é a quantidade de réplicas que o ReplicaSet vai levantar e manter
    replicas: 3
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
          - containerPort: 80    
```

~> DeamonSet  
- Quando falamos de DeamonSets, vem na cabeço os processos que rodam no seu linux do tipo daemon, são processos que rodam em background, e assim que seu sistema é iniciado eles são iniciados também. Como um serviço do mysql, docker e por ai vai.  
- E não é muito diferente aqui no nosso Kubernetes, ao subir o DeamonSet, um pod vai rodar em "background" em um, em mais de um ou em todos os nodes do seu cluster. Com isso podemos usa-lo para subir um pod com prometheus por exemplo, para fazer a parte de monitoramento do cluster.
- E para entender mais de como funciona esse tal do DeamonSet é só olhar a doc oficial do Kubernetes sobre [DEAMONSETS](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)  

~> Job  
- O Pod também é usado quando subimos um objeto do tipo Job. O Job é responsável por criar e gerenciar Pods definidos em um template na especificação do job. Em geral, esses Pods executam até serem concluídos com sucesso, caso aconteça uma falha o controlador do job vai levantar outro pod, com base no template do pod presente.  
- Para entender mais sobre, como funcionam os jobs, basta olhar na doc oficial do Kubernetes sobre [JOBS](https://kubernetes.io/docs/concepts/workloads/controllers/job/)   

Agora que conseguimos subir um pod, vamos entender de que maneiras podemos usar esse pod

### $ Como funciona a comunicação entre pods?

Os pods podem se comunicar facilmente tanto estando no mesmo node, quando em nodes diferentes, isso por causa de que cada pod recebe um ip ao ser criado. Porém não é muito inteligente usar o ip do pod criado, para fazer a conexão entre dois pods, pois os pods foram feitos para serem efêmeros, sendo assim, toda vez que um pod for derrubado e levantado em seguida, ele vai receber um novo ip, assim perdendo a conexão entre os pods.  

Para mantermos esta conexão podemos apontar um service para o pod ou para um deployment, assim não vamos perder a conexão entre os pods. A comunicação dentro do cluster por hostname, só é possível através do servidor DNS que roda como um serviço Kubernetes. Podemos fazer a comunicação da seguinte maneira:  
  - Namespace default ~> `{serviceName}.svc.cluster.local`
  - Em outro namespace ~> `{serviceName}.{namespaceName}.svc.cluster.local`

Dessa maneira é possível fazer a comunicação entre pods, tanto no mesmo node, quanto em node diferentes. Para que tudo isso funcione, é preciso que no seu cluster tenha instalado ou o `kube-dns`, que é o serviço de DNS nativo do k8s, ou configurar o core dns como o serviço de dns principal do seu cluster, vou deixar alguns links no final do artigo para quem quiser fazer a configuração.  

### $ Conclusão  
  
Neste post conseguimos entender um pouco sobre o Pod, entendemos um pouco de o que é um pod, maneiras diferentes de como outros objetos do kubernetes usam ele e também uma breve passagem sobre a comunicação entre pods.  
Na parte dois desse post, vou falar mais e adentrar mais sobre a network entre pods, e persistência de dados para um pod.  
Agradeço quem leu todo o post, ressaltando que esse post é uma via de estudo que eu encontrei, e decidi, compartilhar com qualquer um que queira entender um pouco de Kubernetes assim como eu.  
  
Até o próximo post!!!
