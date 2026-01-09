# FIAP Cloud Games - Infraestrutura

Este repositório contém os manifestos Kubernetes e a documentação de arquitetura para o projeto FIAP Cloud Games. O objetivo desta fase é garantir escalabilidade, resiliência e comunicação assíncrona entre microsserviços rodando em cluster.

## Arquitetura

O sistema é composto por três microsserviços rodando no Azure Kubernetes Service (AKS):

1. **Users API**: Gerenciamento de identidade e autenticação.
2. **Games API**: Catálogo de jogos e orquestração de pedidos de compra.
3. **Payments API**: Gestão de carteira (Wallet) e processamento de pagamentos.

A comunicação entre *Games* e *Payments* é assíncrona, utilizando **Azure Service Bus** com **MassTransit**.

## Tecnologias

* **Cloud Provider**: Microsoft Azure
* **Orquestração**: Kubernetes (AKS)
* **Mensageria**: Azure Service Bus
* **Banco de Dados**: Azure SQL Database / SQL Server em Container
* **Monitoramento**: Logs de Container (stdout) e métricas de Pod.

## Estrutura do Cluster

Os manifestos estão configurados para criar:
* **Deployments**: Para gerenciar os Pods de cada API (.NET 8).
* **Services**: ClusterIP para comunicação interna e LoadBalancer para exposição externa (Ingress).
* **ConfigMaps/Secrets**: Para injeção de ConnectionStrings e credenciais.

## Como Executar (Deploy)

Pré-requisitos: `kubectl` configurado para o contexto do Azure e acesso ao Cluster.

1. Configure as Secrets (Service Bus e Banco de Dados):
   kubectl apply -f secrets.yaml

2. Aplique as configurações e serviços:
    kubectl apply -f configmaps.yaml
    kubectl apply -f services.yaml

3. Realize o deploy das aplicações:
    kubectl apply -f deployment-users.yaml
    kubectl apply -f deployment-games.yaml
    kubectl apply -f deployment-payments.yaml

4. Verifique o status dos pods:
    kubectl get pods