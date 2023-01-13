
# Setting up a new docker-desktop cluster from scratch
```
flux bootstrap github \
    --context=docker-desktop \
    --owner=astevko \
    --repository=astevko/flux2-kustomize-helm-example \
    --branch=test-secrets \
    --personal \
    --path=clusters/docker-desktop
```
## copy staging to docker-desktop
```
cp clusters/staging/infrastructure.yaml clusters/dev
cp clusters/staging/apps.yaml clusters/dev
```