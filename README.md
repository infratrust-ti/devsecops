# 🚀 Infraestrutura GitOps Organizacional -- Kubernetes em Alta Disponibilidade (v1.1)

## 📌 Visão Geral

Esta versão evoluída da plataforma consolida um modelo **CI + GitOps +
DevSecOps enterprise**, incluindo:

-   🔐 Segurança integrada (SAST, SCA, DAST)
-   🔁 Promotion controlada entre ambientes
-   📦 Imagem imutável promovida sem rebuild
-   🏛 Governança organizacional via Git
-   📊 Qualidade de código validada automaticamente
-   📈 Deploy determinístico e auditável

------------------------------------------------------------------------

# 🏗 Arquitetura Geral

A solução é baseada em um cluster **Kubernetes RKE2 em Alta
Disponibilidade**, com CI desacoplado do deploy e Git como fonte única
da verdade.

## 📊 Diagrama Arquitetural

``` mermaid
flowchart LR

    Dev[Developer Commit] --> Tag[Tag vX.Y.Z]
    Tag --> Build[Build - Kaniko]
    Build --> Harbor[Harbor Registry]

    Harbor --> Security[Security Validation]
    Security -->|SonarQube| Sonar[SAST + Quality Gate]
    Security -->|Trivy| Trivy[Image Scan]
    
    Security --> GitOpsDEV[GitOps Update - values-dev.yaml]
    GitOpsDEV --> PR[Pull Request GitOps]
    PR --> MergeDEV[Merge Main GitOps Repo]

    MergeDEV --> Argo[ArgoCD Sync]
    Argo --> K8s[Kubernetes Cluster RKE2 HA]

    K8s --> Validation[Post Deploy Validation]
    Validation --> DAST[OWASP ZAP - DAST]

    DAST --> Promote[Manual Promotion Workflow]
    Promote --> Homol[Update values-homol.yaml]
    Promote --> Prod[Update values-prod.yaml]

    Homol --> Argo
    Prod --> Argo
```

------------------------------------------------------------------------

# 🔧 Componentes da Plataforma

## 🧱 Infraestrutura

-   Kubernetes RKE2 em HA
-   Ingress NGINX
-   cert-manager (TLS automático)
-   Longhorn (Storage distribuído)
-   Harbor (Registry privado)

## 🔐 DevSecOps

-   SonarQube -- SAST + Quality Gate
-   Trivy -- Scan de imagem container
-   OWASP ZAP -- DAST pós deploy

## 🔁 GitOps

-   ArgoCD monitorando repositório declarativo
-   Deploy via Pull Request
-   Drift controlado
-   Auto Sync habilitado

## 📊 Observabilidade

-   Prometheus
-   Grafana
-   Loki

------------------------------------------------------------------------

# 🔄 Modelo de Promotion

A imagem é construída apenas uma vez:

harbor.lab.local/developer/chargeplus:vX.Y.Z

Promotion entre ambientes ocorre via alteração de values:

-   values-dev.yaml
-   values-homol.yaml
-   values-prod.yaml

Sem rebuild. Sem alteração manual no cluster. Sem acesso direto ao
Kubernetes.

------------------------------------------------------------------------

# 🏛 Princípios Arquiteturais

-   Git como fonte da verdade
-   Imagem imutável
-   Separação entre CI e Deploy
-   Promotion controlada
-   Segurança integrada desde o commit
-   Deploy auditável e previsível

------------------------------------------------------------------------

# 🎯 Benefícios

✔ Rastreabilidade completa\
✔ Segurança em múltiplas camadas\
✔ Redução de risco operacional\
✔ Rollback simples\
✔ Escalável para múltiplas aplicações\
✔ Arquitetura pronta para ambientes regulados

------------------------------------------------------------------------

# 🏁 Conclusão

A Infraestrutura GitOps Organizacional v1.1 representa uma arquitetura
moderna e enterprise-ready, unindo:

-   CI desacoplado
-   GitOps determinístico
-   DevSecOps integrado
-   Promotion formal por ambiente
-   Kubernetes em alta disponibilidade

Um modelo aplicável a ambientes corporativos, setor financeiro e
organizações que exigem governança e auditoria contínua.
