
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

## Observe the secret is mounted on the test app
```
% kubectl exec -it test-secrets-8685dc496f-85htf -- /bin/sh
/ # cat /opt/secrets2/etherium-api-key
xxxxxxx
```


# Note This method of securing keys using bitnami's sealed-secrets exposes the unencrypted secret to the k8s control plane.
```
andrewstevko@Trugard-Stevko docker-desktop % kubectl get secrets
NAME               TYPE     DATA   AGE
etherium-api-key   Opaque   1      6m49s
my-secret          Opaque   1      54m
andrewstevko@Trugard-Stevko docker-desktop % kubectl get sealedsecrets.bitnami.com
NAME               AGE
etherium-api-key   54m
my-secret          54m
andrewstevko@Trugard-Stevko docker-desktop % kubectl get secrets etherium-api-key -o yaml | grep api
apiVersion: v1
  etherium-api-key: UTQ0OUpYSUJUVTZIUEtQQkoyVTkxU0pXVThCTTY2NkU0NQ==
  name: etherium-api-key
  - apiVersion: bitnami.com/v1alpha1
    name: etherium-api-key
```

