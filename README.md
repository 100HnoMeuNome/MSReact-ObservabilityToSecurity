# MSReact-ObservabilityToSecurity

🚨 Cuidado! Se você já possui uma conta na Azure, esse lab pode aumentar o seu custo, não faça isso no seu ambiente produtivo nem da Azure nem do Datadog.

## Criando uma conta no Datadog

O Datadog é Saas é então você precisa apenas criar uma conta e não precisa de cartão de crédito.

- [Getting Started with Datadog Sites](https://www.datadoghq.com/technical-enablement/sessions/)
- [Criando uma conta no Datadog](https://us3.datadoghq.com/account/login?redirect=f)

## Fazendo o deploy de uma Aplicação com Kubernetes (Monitoramento da Infra)
Existe um template pronto para utilizarmos não iremos reinventar a roda vamos usar esse mesmo para analisar os eventos nele. 

[Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using an ARM template](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-rm-template?tabs=azure-cli)

## Instalando o Datadog agent

Primeiro você precisa criar o seu cluster K8S se conectar nele e instalar o agente.
Na console do Datadog em Integration > Agent > Kubernetes tem um exemplo de como instalar, nesse
[link](https://github.com/DataDog/helm-charts/blob/main/charts/datadog/values.yaml) tem o arquivo completo do agent para você habilitar os modulos

## Deploy de uma aplicaçao simples (Monitoramento da APM)

O cluster já está criado e o agente já está instalado. Agora iremos fazer o deploy de uma segunda aplicação.

Execute o comando:
```
kubectl apply -f aks-net.yaml
```

Vamos ver o ip externo:
```
kubectl get svc
```
![image](https://github.com/user-attachments/assets/bb313d40-8651-4696-ad8d-91864b10af5d)

Você pode utilizar o Postman para gerar eventos, ele tem uma versão na web o link está [aqui](https://web.postman.co/)

Insira o endereço no método get e você verá o resultado.

![image](https://github.com/user-attachments/assets/fd32083b-d1ed-4d80-8c80-45dbc56bf694)

Na console do Datadog em APM > Trace Explorer podemos ver os eventos

![image](https://github.com/user-attachments/assets/b186c4f7-d02a-447c-b7a7-a3a4c23ff517)

## Monitorando um Serviço

Agora que já fizemos o deploy do nosso serviço podemos configurar um monitor.

Você pode começar por aqui Monitors > Templates 

## Adicionando uma camada de Proteção no AKS

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

Com essas flags habilitaddas iremos analisar vulnerabilidades no cluster, atividades suspeitas e melhores práticas de segurança.

## Gerando eventos de Segurança no AKS

Vamos criar um arquivo chamado shell-demo.yaml, o documento de referência esta [aqui](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container)

```
apiVersion: v1
kind: Pod
metadata:
  name: shell-demo
spec:
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  hostNetwork: true
  dnsPolicy: Default
```

Depois de criarmos o arquivo precisamos executa-lo e acessar o pod em modo interativo. 

```
kubectl apply -f shell-demo.yaml
```
```
kubectl exec --stdin --tty shell-demo -- /bin/bash
```
Na console do Datadog em Security > Cloud Security Management > Signals Explorer iremos ver o evento.

![image](https://github.com/user-attachments/assets/8750ab2a-33dc-4832-9eae-5483c76a58a2)

Outro comando que podemos executar é curl no IMDS, ainda dentro do container execute o comando:

```
curl http://169.254.169.254/metadata/instance/compute?api-version=2021-01-01&format=json
```
## Gerando eventos de Segurança na Aplicação 

Para termos um pouco mais de emoção podemos utilizar o [Nikto](https://github.com/sullo/nikto) (scan de vulnerabilidade). **Não execute scans sem autorização é crime!**

```
nikto -h http://meu-ip
```
Em Security > Application Security > Signal Explorer vamos ver o evento abaixo do nosso scan

![image](https://github.com/user-attachments/assets/6f6eb269-4e67-4913-801a-eae815a00da3)

## É isso ai!

A ideia do repositório é compartilhar os códigos para que você possa replicar quando necessário, a Observabilidade e Segurança são importantes para entregar o melhor para nossos clientes.

## Treinamentos adicionais gratuitos
- [Foundation Enablement sessions](https://www.datadoghq.com/technical-enablement/sessions/)
- [Datadog Learning Center](https://learn.datadoghq.com/)
