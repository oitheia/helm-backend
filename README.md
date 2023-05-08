# Documentação Helm Backend

Documentação do repositório do helm dos microsserviços em Go + aldeia e chronos  https://github.com/oitheia/helm-backend. 

Para gerenciar os templates do helm foi criado um repositório público no github que armazena os templates e o chart do helm. Para atualizar o chart é necessário seguir esses passos:

# Como atualizar chart

```
git clone
cd helm-backend
#Antes de gerar o pacote atualizar a versao do chart no arquivo chart.yaml
helm package ~/folder #Pasta com o template e arquivo chart.yaml e gera o *.tgz no diretorio atual
helm repo index . #Atualiza o index.yaml
git add .
git commit -m "update repo"
git push
```

# Pipeline

```yaml
# Adiciona o repositório do github
- run:
    name: Install helm repo from github
    command: helm repo add helm-backend https://raw.githubusercontent.com/oitheia/helm-backend/main/ && helm repo update
# Executa o helm upgrade com o repositório adiciona anteriormente helm-backend/service
- run:
    name: Helm Upgrade
    command: helm secrets upgrade ${CIRCLE_PROJECT_REPONAME} helm-backend/service --install ....
```

# Documentação
## Template:

- Deployment
- Horizontal Pod Autoscaler
- Ingress
- Namespace
- Secret
- Service
- Service Account

## Values:

autoscaling: Parâmetros do HPA(HorizontalPodAutoscaler)

enabled: Habilitar ou não o HPA

minReplicas: Quant. mínima de réplicas 

maxReplicas: Quant. máxima de réplicas 

targetCPUUtilizationPercentage: Porcentagem de uso de CPU

replicaCount:  Números de réplicas do deployment

appPort: Porta de execução da aplicação, estar de acordo com a variável “APP_PORT” ou “SERVER_PORT”

nameOverride: Substituir o nome dos recursos ao invés do nome do release no helm Ex: Se o nome do release for “test” o nome dos recursos ficaram como “test-service”. Já se o valor do “nameOverride”  for setado, por exemplo como “testando”, os recursos ficaram com o nome de “testando-service”

fullnameOverride: Substitui o nome de todos os recursos pelo valor que for definido Ex: se o valor for definido como “teste”, todos os recursos terão o nome “teste”. 

healthPath: HealthPath para o liveness e readiness probe, para verificar a saúde do pod

serviceAccount: Service account para o deployment associado a uma role na AWS, para mais detalhes([Configuring a Kubernetes service account to assume an IAM role - Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/associate-service-account-role.html))

enable: true para criar o service account

roleArn: Se o enable estiver como true é obrigatório indicar o arn da role que será associado pelo service account

resources: Definir os recursos de deployment

limits: Limite de cpu e memória

cpu: Valor limite de cpu

memory: Valor limite de memória

requests: Pedido de memória e cpu

cpu: Valor de cpu

memory: Valor de memória

ingress: Ingress para expor a aplicação com o load balancer + route53 para criar o DNS. Integração usando o ALB Controller([Set up an ALB using the AWS Load Balancer Controller on an Amazon EC2 node group in Amazon EKS | AWS re:Post (repost.aws)](https://repost.aws/pt/knowledge-center/eks-alb-ingress-controller-setup))

annotations: Annotations para a configuração do ALB Controller (Não precisa editar), para mais detalhes de cada annotation([Annotations - AWS Load Balancer Controller (kubernetes-sigs.github.io)](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/guide/ingress/annotations/))

hosts:

host: Nome do record no Route 53, DNS usado para acessar a aplicação
