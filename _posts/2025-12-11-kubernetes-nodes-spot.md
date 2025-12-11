---
layout: post
title: "Spot nodes em cluster produtivo, por que usar?"
date: 2025-12-11 00:00:00 -0300 
categories: [kubernetes]
tags: [kubernetes, spot, nodes, cloud]
author: guilherme
image: assets/images/4.jpg
---

#### Custos de clusters em cloud

Bom, não é novidade pra ninguém que você ter um cluster dentro de uma cloud pública da vida (AWS, GCP, Azure, IBM), tem suas vantagens, mas também não é de graça. O custo de um cluster simples/pequeno em uma cloud pública, pode chegar quase na casa dos R$ 1.000,00 se você somar tudo, que seria algo mais ou menos assim: 

 Rede + Compute Instace + Cluster Manager ou Master Node + Registry.  

Em cima desse cálculo, conseguimos exugar o custo quando utilizamos o provisionamento do tipo Spot. Com esse modo conseguimos reduzir os custos por volta de uns 50% ou mais (Em cima de Compute instace). Porém, a cada escolha, uma renûncia. As máquina Spot podem ser desligadas a qualquer momento, sem nenhum tipo de SLA, somente com um aviso de 30 segundos. Isso acontece pois os recursos utilizados para subir as instâncias, sem sua parte são:

- Provedores de nuvem precisam manter capacidade de reserva disponível para atender a qualquer aumento na demanda dos clientes [Google](https://docs.cloud.google.com/compute/docs/instances/spot)
- Servidores físicos que não estão sendo usados por clientes que compraram instâncias on-demand, reservadas ou com outros compromissos contratuais
- Capacidade ociosa que varia por região, zona de disponibilidade e tipo de máquina
#### O que colocar no spot node?

Eis a questão, o que danado eu vou colocar nesses nodes, se eu não tenho a segurança que eles vão ficar de pé? Se você se perguntou isso, calma meu jovem padawan! 
Tudo vai depender de uma simples pergunta: 

"Meu serviço pode interrompido abruptamente e/ou demorar alguns minutos para voltar a rodar?"

Cara se sua resposta é sim, pode subir nodes spot e ser feliz! Pois você vai conseguir diminuir o custo do seu cluster.

Mas voltando ao tópico, podemos subir alguns serviços no cluster, que fazem sentido quando utilizamos nodes spot. Vai ai alguns exemplos:
- Serviços com auto-scaling agressivo que compensa a demora
- Workers de filas com DLQ e retry, onde atraso é aceitável
- Serviços com circuit breakers que automaticamente rotam tráfego
- Processamento batch onde "eventualmente vai rodar" é suficiente
E ainda digo mais, se você está em uma empresa que não pode gastar tanto, você pode subir uma cluster de teste quiçá desenvolvimento com nodes somente Spot. Assim você consegue reduzir os custos e ter um cluster em cloud pronto pra ser utilizado. Somando isso com helm + registry open source e outras estratégias você consegue uma boa economia no final das contas.

#### Node Spot em cluster produtivo?
Sim meu jovem, da pra subir nodes spot em clusters produtivos. Pra isso temos algumas abordagens do próprio kubernetes pra isso:
##### 1. **Taints e Tolerations (Mais Seguro)**

A abordagem mais robusta é usar taints nos nodes spot combinado com tolerations nos pods:

**Taint nos nodes spot:**

```bash
# Aplicar taint em todos os nodes spot
kubectl taint nodes <node-spot> workload=spot:NoSchedule
```

**Toleration no pod:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker-batch
spec:
  template:
    spec:
      tolerations:
      - key: "workload"
        operator: "Equal"
        value: "spot"
        effect: "NoSchedule"
      # Isso sozinho permite rodar em spot, mas não garante
```

**Observação:** Isso permite rodar em spot, mas o pod ainda pode cair em nodes on-demand se eles não tiverem taint.
##### 2. **Node Affinity (A que eu mais gosto)**

Para **garantir** que só suba em spot, combine com nodeAffinity:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker-batch
spec:
  template:
    spec:
      tolerations:
      - key: "workload"
        operator: "Equal"
        value: "spot"
        effect: "NoSchedule"
      
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node.kubernetes.io/instance-type
                operator: In
                values:
                - spot
              # OU usar label customizada
              - key: workload
                operator: In
                values:
                - spot
```

**Obersvação** Lembra de dar uma olhada nas labels dos teus nodes, pois pode mudar de cloud pra cloud.

#### Ok, mas quanto consigo reduzir meus custos?

Vou te mostrar com um exemplo de uma calculadora da GCP o quanto tu consegue reduzir somente mudando pra spot intance.

**Custo com node On Demand:**

![Image description]({{ site.baseurl }}/assets/images/post-3-ondemand.png)

**Custo com nodes Spot:**

![Image description]({{ site.baseurl }}/assets/images/post-3-spot.png)

Somente com essa mudança tem um savings de $ 1,243.69 ou seja mais de 70% de economia no seu cluster. 

#### Dica!!!!!!

Em caso de utilizar nodes spot ou até mesmo sobdemanda. Mas essa dica é precisamente para nodes spot. Distribua seus nodes spot por diferentes AZs e com diferentes instace types. Dessa maneira vai ser mais difícil você ter todos os seus nodes spot indisponíveis, quando uma zona estver cheia e um node seu cair, em outra zona pode estar normal. Diferente de uma estratégia de jogar tudo em uma única AZ, desse maneira todos os seus nodes podem ficar indisponíveis ao mesmo tempo.


Bom, creio que é isso pessoal, até o próximo post!
