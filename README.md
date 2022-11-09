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

    - Entender os momentos do seu sistemas

    - Tomar decisão do que fazer quando com o pod

2. Criando endpoint Healthz

    - Alterando o app server.go adicionando função de duração de execução dos pods e setando que se o POD ficar mais de 25 segundos aparecerá um erro 500 caso contrário 200.

3. Liveness na prática

    - 3 tipos de Liveness: HTTP, COMMAND e TCP

    - Inserindo livenessProbe dentro do deployment

4. Entendendo Readiness

    - Verifica se sua aplicação está pronta e só manda trafego após verificação

    - Inserção do readiness e suas opções dentro do deployment

5. Combinando Liveness e Readiness

    - Readiness desvia o trafego e o Liveness reinicia o POD

6. Trabalhando com startupProbe

    - Inserindo startupProbe no deployment

    - startupProbe seta limite de quanto sua aplicação pode demorar para subir, no exemplo ele irá verificar durante aproximadamente 90 segundos e aplicação ficando pronta o kubernetes libera o readiness e o liveness.

    - Recomendação de sempre utilizar o startupProbe

## Resources e HPA

1. Instalando metrics-server

    - Overview sobre o metrics-server

    - Instalando o metrics-server e modificando o deploy para funcionar corretamente.

    - https://github.com/kubernetes-sigs/metrics-server

    - Baixar o deployment do metrics-server e adicionar a linha `- --kubelet-insecure-tls`

2. Entendendo utilização de Resources

    - Limitação de recursos dos PODs (Reservando recursos para o POD)

    - Editando o deployment, inserindo o bloco de `resources`

    - Request = Reserva de recursos -> limits = Limite de recursos

3. Aplicando deployment com resources

    - Visualização de recursos com o comando `kubectl top pod NOMEDOPOD`

4. Criando e configurando HPA

    - Horizontal POD AutoScaler

    - Criação do arquivo `hpa.yaml`

5. Versão da imagem para o teste de stress

    - Alteração no app server.go tirando `< 30` do código.

6. Atualização no comando do Fortio

    - Outra nota informando sobre um atributo que não é mais suportado no kubernetes

    - Retirar do comando o trecho `--generator=run-pod/v1`

7. Teste de stress com fortio

    - Realizando o comando do Fortio para Stress `kubectl run -it fortio --rm --image=fortio/fortio -- load -qps 800 -t 120s -c 70 "http://goserver-service/healthz"`

    - Vendo os PODs escalarem com os comandos `kubectl get hpa` e `kubectl get pods`

## Statefulsets e volumes persistentes

1. Entendendo volumes persistentes

    - Statelss sem estado guardado na aplicação

    - Volume, possível criar um volume dentro do container

    - Pull de Storage persistente e dinamico qtd liberada para o container

    - StorageClass: Claim -> StorageClass -> Disponibilizar o espaço necessário

    - ReadWriteOnce => Pode ler e gravar somente quando os pods estão no mesmo NODE

2. Criando volumes persistentes

    - Criação do arquivo pvc.yaml

    - `kubectl get pvc`

    - Editando e inserindo pvc dentro do deployment

    - Montando o pvc

3. Entendendo Stateless vc Stateful

    - Overview sobre Stateful quando trabalhamos por exemplo com banco de dados e possamos criar pods e nodes de forma organizada.

4. Criando StatefulSet

    - Criando o arquivo statefulset.yaml

    - Criando 3 replicas de um mysql

    - Verificando a quantidade de replicas aumentando e diminuindo de forma organizada.

    - Comando scale `kubectl scale statefulset mysql --replicas=7`

5. Criando headless service

    - Apontamento de DNS - Escolher para qual POD queremos enviar a requisição.

    - Criando o arquivo mysql-service-h.yaml

6. Criando volumes dinamicamente com statefulset

    - Adicionando PVC ao statefulset

    - Editando arquivo statefulset

7. Devo usar meu banco de dados no kubernetes

    - Opinião sobre usar ou não banco de dados no kubernetes

    - Dar preferencia a bancos de dados gerenciados (como RDS na AWS ou CloudSQL no GCP)

## Ingress

1. Visão geral

    - Ingress ponto único de entrada na sua aplicação

    - Faz o "roteamento" para os microserviços

2. Configurando aplicação no GKE

    - Subiu todos os arquivos com `kubectl apply -f k8s/`

3. Instalando ingress nginx controller

    - Instalando via Helm <https://kubernetes.github.io/ingress-nginx/deploy/>

4. Configurando Ingress e DNS

    - Criando o arquivo `ingress.yaml`

    - Criando paths de acesso a aplicação em `path`

## Cert-manager

1. Instalando cert manager

    - Link de doc para instalação `https://cert-manager.io/docs/installation/kubectl/`

2. Configurando e emitindo certificado

    - Criando o arquivo `cluster-issuer.yaml`

    - Configurando para utilizar o let's encrypt para criação e gerenciamento dos certificados.

    - Inserindo `annotations` dentro do arquivo `ingress.yaml` e inserido configuração do let's encrypt bloco `tls`

    - Comando para verificação de certificados `kubectl describe certificate SECRETNAME`

## Namespaces e Service Accounts

1. Namespaces

    - Namespace serve para separar projetos podendo separar também requisitos de segurança e até mesmo separar hardware para cada um.

2. Contextos por namespace

    - Criando um diretório com um novo deployment `namespaces/deployment.yaml`

    -










