# Repositório criado para o curso de Kubernetes da FullCycle

## Iniciando com Kubernetes

1. Introdução ao Kubernetes

    - Explicação geral sobre o Kubernetes

2. Instalando Kind

    - Kind é uma ferramenta para executar clusters locais do Kubernetes usando "nodes" de conteiner Docker.

    - Site: https://kind.sigs.k8s.io/

    - Instalado kind via "chocolatey"

    - Também é necessário instalar o kubectl, ferramenta para conexão e gerenciamento dos clusters

    - Site: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

    - Foi instalado via "Chocolatey"

3. Criando primeiro cluster com kind

    - Primeiros comandos no kind

4. Criando cluster multi node

    - Criação do arquivo kind.yaml, arquivo utilizado para

    - criação de vários clusters

5. Mudança de contexto e extensão do VSCode

    - Dica para instalação de uma extensão no VSCode para a troca mais fácil entre os clusters

## Primeiros passos na prática

1. Criando aplicação exemplo e imagem

    - Criação de um app em GoLang chamado "server.go"

    - Criação de um arquivo Dockerfile para build de imagem para utilização do app em Go

    - Feito push da imagem criada para o DockerHub

2. Trabalhando com Pods

    - Comandos do kubectl para criação do pod

    - Criação do arquivo pod.yaml

    - Realização do comando kubectl port-for

3. Criando primeira ReplicaSet

    - Trabalhando com ReplicaSet

    - Criação do arquivo replicaset.yaml

    - Dentro desse arquivo criando a quantidade de replicas de um pod

    - Se eu apagar um pod devido a configuração do replicaset automaticamente se cria outros pods

4. Problema do ReplicaSet

    - Criado nova versão do server.go e atualizado arquivo do ReplicaSet e os pods não foram atualizados

    - Pod só atualiza quando ele é deletado e recriado

5. Implementando Deployment

    - Deployment -> ReplicaSet -> Pod

    - Criação do arquivo deployment.yaml

    - Atualizando a versão do server.go dentro do arquivo deployment ele faz a atualização dos pods automaticamente

    - Deployment cria novos replicasets

6. Rollout e Revisões

    - Historico de versões

    - Voltando para versões anteriores (undo e --to-revision=*)

## Services

1. Entendendo o conceito de services

    - Explicando o conceito de services, atua como se fosse um load balancer encaminhando a sessão para os pods.

2. Utilizando ClusterIP

    - Criação do arquivo service.yaml

    - Fazendo port-forward para o service criado

3. Diferenças entre Port e targetPort

    - port do arquivo service é a porta do próprio service e targetPort é a porta para qual será encaminhada para o conteiner

4. Utilizando proxy para acessar API do Kubernetes

    - Comando kubectl proxu mostrando a API do Kubernetes

5. Utilizando NodePort

    - NodePort abre uma porta em todos os nodes

    - Alteração do arquivo service.yaml

    - Acessando o IP do node na porta 30001, será encaminhado para a porta 80 do service e que encaminhará para a porta 7000 dos pods

6. Trabalhando com LoadBalancer

    - Gera um IP externo para acesso a aplicação

## Objetos de configuração

1. Entendendo objetos de configuração

    - Conceito inicial dos objetos de configuração

2. Utilizando váriaveis de ambiente

    - Colocando váriaveis no app e fazendo o push para o DockerHub.

    - Modificando o arquivo de deploy.

3. Váriaveis de ambiente com ConfigMap

    - Criando confimap-env para configuração de váriaveis

    - Alterando arquivo de deployment indicando váriaveis contidas dentro do ConfigMap

    - Duas formas de declaração de váriaveis, `valueFrom` dentro de `env` ou um `envFrom` apontando o arquivo de configMap.

4. Injetando ConfigMap na aplicação

    - Criando configmap-family para injeção de váriaveis pela aplicação

    - Novo build de imagem e push para o DockerHub (v7)

    - Montando um volume para envio do configmap-family

    - Montando o volume dentro do deployment

    - Verificando logs do pod `kubectl logs`

    - Executando um pod com o comando `kubectl exec -it NOMEDOPOD -- bash`

5. Secrets e váriaveis de ambient

    - Alterando app adicionando nova função com secret

    - Kubernetes usa por padrão dados sensiveis como senhas em Base64

    - secret vira váriavel de ambiente

## Probes

1. Entendendo Health Check

    -






