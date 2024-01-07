```bash
export PROJ_PATH="terenowe-info/helm"
export BRANCH_NAME="main"

export REPO_URL_RAW="https://raw.githubusercontent.com/${PROJ_PATH}"
export REPO_URL="https://github.com/${PROJ_PATH}.git"

kubectl create namespace argocd-system

helm upgrade \
  --install argocd argo/argo-cd \
  --namespace argocd-system \
  --version 5.51.4 \
  --values "${REPO_URL_RAW}/${BRANCH_NAME}/base/argocd/argocd/values.yaml"

helm upgrade \
  --install argocd-apps argo/argocd-apps \
  --namespace argocd-system \
  --version 1.4.0 \
  --values "${REPO_URL_RAW}/${BRANCH_NAME}/base/argocd/argocd-apps/values.yaml"

kubectl apply -f <(kustomize build "${REPO_URL}/overlays?ref=${BRANCH_NAME}")
```
