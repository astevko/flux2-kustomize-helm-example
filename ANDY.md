
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
## get the public key from the controller
```
kubeseal --fetch-cert \
--controller-name=sealed-secrets \
--controller-namespace=flux-system \
> pub-sealed-secrets.pem
```

## Seal the secret...
```
kubeseal --format=yaml --cert=pub-sealed-secrets.pem \
< etherium-api-key.yaml > etherium-api-key-sealed.yaml
```