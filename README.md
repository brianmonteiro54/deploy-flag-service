# deploy-togglemaster-flag

Repositório GitOps do microsserviço `flag-service` do projeto ToggleMaster.

## Estrutura

- `manifests/`: arquivos YAML consumidos pelo ArgoCD
- `README.md`: documentação do repositório

## Fluxo GitOps

1. O pipeline do repositório da aplicação gera uma nova imagem Docker.
2. A imagem é publicada no registry.
3. A tag da imagem é atualizada em `manifests/deployment.yaml`.
4. O ArgoCD detecta a alteração neste repositório.
5. O ArgoCD sincroniza automaticamente o ambiente.

## Configuração atual

- Namespace: `togglemaster-flag`
- Serviço: `flag-service`
- Porta: `8002`
- Healthcheck: `/health`
- Hostname: `toggle.pt`
- Path público: `/flags`
- Réplicas: `2`

## Variáveis obrigatórias da aplicação

- `DATABASE_URL`
- `AUTH_SERVICE_URL`

## Observações

- O `DATABASE_URL` é montado no `initContainer`.
- O `AUTH_SERVICE_URL` é consumido via Kubernetes Secret materializado pelo `ExternalSecret`.
- As credenciais do banco são materializadas via `ExternalSecret` (RDS managed secret).
- As configurações (`FLAG_DB_HOST`, `FLAG_DB_NAME`, `FLAG_DB_PORT`, `AUTH_SERVICE_URL`) são geridas pelo AWS Secrets Manager no secret `togglemaster/flag-service`.
- O arquivo `deployment.yaml` é o principal ponto atualizado pela pipeline da aplicação quando uma nova imagem for publicada.

## Pré-requisitos no cluster

- NGINX Ingress Controller
- External Secrets Operator
- ArgoCD instalado e configurado
- Secret `aws-credentials` no namespace `togglemaster-flag`
- Secret do RDS disponível no AWS Secrets Manager