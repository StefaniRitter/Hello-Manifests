# Hello-Manifests: Manifestos Kubernetes

## Visão Geral

Este é o Repositório de Configuração (GitOps) para a aplicação `hello-app`. Ele armazena todos os manifestos do Kubernetes que definem o estado desejado da aplicação no cluster.

Seguindo o princípio GitOps, este repositório serve como a única "Fonte de Verdade" que o ArgoCD monitora e sincroniza continuamente.

## Objetivo Principal

Garantir que a configuração do ambiente de execução no Kubernetes (Rancher Desktop) esteja sempre em conformidade com os arquivos YAML contidos neste repositório, automatizando o processo de deploy e atualização da aplicação hello-app.

## Tecnologias

* **ArgoCD**: Agente de Entrega Contínua. Monitora este repositório, detecta alterações e aplica os manifestos no cluster.
* **Kubernetes**: Define o Deployment, Service e a configuração de imagem da aplicação.
* **GitHub Actions**: Aciona as alterações neste repositório, atualizando a tag da imagem do `hello-app` sempre que um novo build é concluído.

## Fluxo de Atualização

A atualização deste repositório é totalmente automatizada pelo pipeline de CI/CD:

1. Um novo código é commitado no repositório de aplicação `hello-app`.
2. O GitHub Actions (no `hello-app`) constrói e faz um push da nova imagem para o Docker Hub.
3. O GitHub Actions, então, faz um commit automático neste repositório (**`hello-manifests`**), atualizando o campo spec.template.spec.containers[0].image no arquivo k8s/deployment.yaml com a nova tag da imagem.
4. O ArgoCD detecta o novo commit e sincroniza o cluster, efetuando o rollout da nova versão do hello-app.

Não são necessárias intervenções manuais no cluster (como kubectl apply).

[Voltar para o Repositório do Código (hello-app)](https://github.com/StefaniRitter/Hello-App)






