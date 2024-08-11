# MSReact-ObservabilityToSecurity

🚨 Cuidado! Se voce já possui uma conta na Azure esse lab pode aumentar o seu custo, nao faça isso no seu ambiente produtivo nem da Azure nem do Datadog

## Criando uma conta no Datadog

O Datadog é Saas é entao voce precisa apenas criar uma conta e nao precisa de cartao de credito.

- [Getting Started with Datadog Sites](https://www.datadoghq.com/technical-enablement/sessions/)
- [Criando uma conta no Datadog](https://us3.datadoghq.com/account/login?redirect=f)

## Fazendo o deploy de uma Aplicação
Existe um template pronto para utilizarmos não iremos reinventar a roda vamos usar esse mesmo para analisar os eventos nele. 

[Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using an ARM template](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-rm-template?tabs=azure-cli)

## Instalando o Datadog agent

Primeiro você precisa criar o seu cluster K8S se conectar nele e instalar o agent.
Na console do Datadog em Integration > Agent > Kuberenetes tem um exemplo de como instalar, nesse [link](https://github.com/DataDog/helm-charts/blob/main/charts/datadog/values.yaml) tem o arquivo completo do agent para você habilitar os modulos

## Monitorando um Serviço

Agora que já fizemos o deplioy do nosso serviço podemos configurar um monitor.

Você pode começar por aqui Monitors > Templates 

## Adicionando uma camada de Proteção

Relacionado a segurança, as flags abaixo precisam estar habilitadas. Dessa forma iremos analisar as atividades do cluster Kubernetes.

```
  securityAgent:
    runtime:
      enabled: true
    compliance:
      enabled: true
  sbom:
    containerImage:
      enabled: true
    host:
      enabled: true
```

## Treinamentos adicionais gratuitos
- [Foundation Enablement sessions](https://www.datadoghq.com/technical-enablement/sessions/)
- [Datadog Learning Center](https://learn.datadoghq.com/)
