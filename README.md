# 🚀 From Scratch to Production: EKS + Jenkins + Terraform CI/CD Pipeline

![EKS CI/CD Architecture](project_19.png)

---

## 📌 Sobre o Projeto

Este projeto demonstra a construção completa de uma arquitetura DevOps moderna, desde o provisionamento da infraestrutura até o deploy automatizado de aplicações em Kubernetes.

A solução implementa:

- Provisionamento de infraestrutura AWS com Terraform
- Criação de cluster Amazon EKS
- Configuração de servidor Jenkins automatizado
- Pipeline CI/CD para infraestrutura e aplicação
- Deploy de aplicação NGINX em Kubernetes
- Estratégia de Apply/Destroy automatizada

O projeto simula um cenário real de produção, aplicando conceitos de automação, segurança, versionamento e entrega contínua.

---

# 🏗️ Arquitetura da Solução

A arquitetura é composta por três camadas principais:

## 1️⃣ Camada de Automação (Jenkins Server)

- EC2 provisionada via Terraform
- Jenkins instalado automaticamente via user_data
- Docker, Terraform, AWS CLI, kubectl, Helm, Trivy e SonarQube configurados
- Pipeline declarativo (Jenkinsfile)

## 2️⃣ Camada de Infraestrutura (Terraform + AWS)

- VPC customizada
- Subnets públicas e privadas
- NAT Gateway
- Amazon EKS Cluster
- Node Groups gerenciados
- Backend remoto em S3 para Terraform State

## 3️⃣ Camada de Aplicação (Kubernetes)

- Deployment NGINX
- Service do tipo LoadBalancer
- Namespace dedicado
- Exposição pública via ELB

---

# 🧠 Decisões Arquiteturais

- Separação entre pipeline de infraestrutura e aplicação
- Backend remoto S3 para controle de estado do Terraform
- Cluster EKS em subnets privadas
- Jenkins provisionado como código (IaC)
- Pipeline parametrizado (apply/destroy)
- Controle de acesso via IAM e EKS Access Entry
- Uso de módulos oficiais do Terraform Registry

---

# ⚙️ Stack Tecnológica

- AWS (EC2, VPC, IAM, S3, EKS, ELB)
- Terraform >= 1.0
- Jenkins
- Docker
- Kubernetes
- Helm
- Trivy
- SonarQube
- Git

---

# 📂 Estrutura do Projeto

```
project-root/
│
├── jenkins_server/
│   ├── backend.tf
│   ├── main.tf
│   ├── data.tf
│   └── install_build_tools.sh
│
├── tf-aws-eks/
│   ├── backend.tf
│   ├── vpc.tf
│   ├── eks.tf
│   └── dev.tfvars
│
├── manifest/
│   ├── deployment.yaml
│   └── service.yaml
│
└── Jenkinsfile
```

---

# 🚀 Stage 1 – Provisionando o Jenkins Server com Terraform

## O que é criado:

- VPC
- Security Group
- EC2 Instance
- Instalação automática de:
  - Jenkins
  - Docker
  - Terraform
  - AWS CLI
  - kubectl
  - Helm
  - Trivy
  - SonarQube

## Execução

```bash
terraform init
terraform apply -var-file=variables/dev.tfvars --auto-approve
```

Após provisionamento:

Acessar:
```
http://<PUBLIC_IP>:8080
```

Configurar Jenkins com plugins sugeridos.

---

# 🚀 Stage 2 – Provisionando o Cluster EKS

Cluster configurado com:

- Subnets privadas
- NAT Gateway
- Node Group gerenciado
- Cluster endpoint público
- Backend remoto S3

Validação local:

```bash
terraform init
terraform validate
terraform plan
```

---

# 🚀 Stage 3 – Pipeline CI/CD com Jenkins

Pipeline inclui:

- Checkout do repositório
- Terraform init
- Terraform validate
- Terraform plan
- Aprovação manual
- Terraform apply ou destroy (parametrizado)

## Pipeline Parametrizado

Parâmetro:

```
action = apply | destroy
```

Execução no Jenkins:

Build with Parameters → selecionar ação.

---

# 🚀 Stage 4 – Deploy da Aplicação NGINX

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
```

## Service

```yaml
apiVersion: v1
kind: Service
spec:
  type: LoadBalancer
```

## Aplicação via Pipeline

```bash
aws eks update-kubeconfig --name my-eks-cluster
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Após deploy:

```bash
kubectl get svc
```

Acessar via EXTERNAL-IP do LoadBalancer.

---

# 🔐 Segurança Implementada

- IAM Credentials armazenadas no Jenkins Credentials Store
- Backend remoto S3 para state seguro
- EKS Access Entry configurado
- Cluster em subnets privadas
- Security Groups restritivos

---

# 📊 Resultados Técnicos

- Infraestrutura 100% como código
- Pipeline CI/CD completo
- Provisionamento automatizado de cluster Kubernetes
- Deploy automatizado de aplicação
- Ambiente reproduzível
- Estratégia de teardown automatizada
- Separação clara entre infraestrutura e workload

---

# 🧹 Teardown Automatizado

Para destruir recursos:

No Jenkins:
```
Build with Parameters → action = destroy
```

Ou localmente:

```bash
terraform destroy --auto-approve
```

---

# 📈 Possíveis Evoluções

- Separar pipeline Infra x Application
- Adicionar Docker Build + Push para ECR
- Implementar GitOps com ArgoCD
- Integrar Prometheus + Grafana
- Configurar HPA (Horizontal Pod Autoscaler)
- Implementar Blue/Green Deployment
- Adicionar integração com Slack
- Implementar Policy as Code

---

# 🎯 Conclusão

Este projeto demonstra uma implementação completa de:

- Infrastructure as Code
- Continuous Integration
- Continuous Delivery
- Kubernetes Deployment
- Cloud Automation

Ele simula um ambiente real de produção, aplicando práticas modernas de DevOps e Cloud Engineering.

---

# ⭐ Se este projeto foi útil

Considere:

- Dar uma estrela ⭐
- Compartilhar com sua rede
- Contribuir com melhorias

---

> Projeto demonstrando automação completa de infraestrutura e entrega contínua de aplicações em ambiente Kubernetes na AWS.
