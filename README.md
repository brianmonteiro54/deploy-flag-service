# Deploy Flag Service

Repositório GitOps contendo os manifestos Kubernetes do **Flag Service** da plataforma ToggleMaster.

## Visão Geral

Este repositório é monitorado pelo **ArgoCD** e contém exclusivamente os manifestos declarativos de deploy do Flag Service. Qualquer alteração nos manifests dispara uma sincronização automática no cluster EKS.

A tag da imagem Docker é atualizada automaticamente pelo pipeline de CI do repositório [`flag-service`](https://github.com/brianmonteiro54/targeting-service) a cada push na branch `main`.

## Manifestos

| Arquivo | Descrição |
|---|---|
| `deployment.yaml` | Deployment com rolling update, probes e init container para build da DATABASE_URL |
| `service.yaml` | Service ClusterIP na porta 8002 |
| `ingress.yaml` | Ingress para exposição externa |
| `secretstore.yaml` | SecretStore para integração com AWS Secrets Manager |
| `externalsecret.yaml` | ExternalSecret para injeção segura de credenciais do RDS |

## Fluxo GitOps

```
flag-service (CI) → push na main → pipeline builda imagem → atualiza tag aqui → ArgoCD sincroniza → EKS
```

## Estrutura

```
├── manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── secretstore.yaml
│   └── externalsecret.yaml
└── README.md
```

## Contexto

Este repositório faz parte do **Tech Challenge Fase 3 — POSTECH**, que implementa práticas de IaC (Terraform), CI/CD com DevSecOps e GitOps (ArgoCD) para os 5 microsserviços do ToggleMaster, rodando em AWS EKS via AWS Academy.
