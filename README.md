# 🚀 Infraestrutura GitOps Organizacional -- Kubernetes em Alta Disponibilidade (v2.0)

## 📌 Visão Geral

Esta versão evoluída da plataforma consolida um modelo **CI + GitOps +
DevSecOps**, incluindo:

-   🔐 Segurança integrada (SAST, SCA, DAST)
-   🔐 Templates reutilizáveis
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

# 🏛 Evidências

<img width="1593" height="780" alt="sonar" src="https://github.com/user-attachments/assets/0739093d-49de-494e-9926-cb3a03536fca" />
<img width="1663" height="1082" alt="Captura de tela de 2026-02-23 16-18-21" src="https://github.com/user-attachments/assets/7430c440-6692-4108-8b1d-45921d3ff0fa" />
<img width="1746" height="957" alt="Captura de tela de 2026-02-23 16-19-08" src="https://github.com/user-attachments/assets/328f6adc-4f28-49fd-9383-73192342bc83" />
<img width="1655" height="670" alt="Captura de tela de 2026-02-23 16-34-27" src="https://github.com/user-attachments/assets/313bde49-1c8b-4c18-b498-4bacd2da2f84" />
<img width="1655" height="1137" alt="Captura de tela de 2026-02-23 17-59-09" src="https://github.com/user-attachments/assets/6c8c98d6-a5d0-445a-81a2-7b02698fc4f2" />


------------------------------------------------------------------------

# 🏁 Conclusão

A Infraestrutura GitOps Organizacional v2.0 representa uma arquitetura
moderna e enterprise-ready, unindo:

-   CI desacoplado
-   GitOps determinístico
-   DevSecOps integrado
-   Promotion formal por ambiente
-   Kubernetes em alta disponibilidade

Um modelo aplicável a ambientes corporativos, setor financeiro e
organizações que exigem governança e auditoria contínua.


