# k8s-learning

## Considerations

I had more success using the Bitnami images in favor of the official images from
some projects.

The learning curve in this kind of project is a little high for me right now, but hey, it's working !

I'm using Google Cloud to manage my resources. Its simple, easy and has many features (also not so expensive)

This instructions assume that the cluster is a new one.

With this, i can deploy applications facing the web with proper features (DNS and SSL)

## Some variables

# a@a.com
--set domainEmail=

# apps.a.com
--set hostname=

# Google Oauth ID: 123456789-abcdefg.apps.googleusercontent.com
--set clientID=

# Google Oauth secret: abcdefgh123940k
--set clientSecret=

# Random string that will be encoded to a base64 string.
# Please make it 32 bytes length base64 string (no more or less, 22 chars len string)
--set cookieSecret=

## Nginx Controller

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx -f "ingress-nginx-values.yaml"
```

## External DNS

```bash
kubectl create secret generic external-dns --from-file=external-dns-sa.json
helm install external-dns --set hostname=<domain> bitnami/external-dns -f "external-dns-values.yaml"
```

## Cert-Manager

```bash
kubectl create namespace cert-manager
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.12.0/cert-manager.yaml
kubectl create --set domainEmail=<email> -f "cert-manager-issuer.yaml"
```

## Kubeapps - Google OAuth

```bash
helm install kubeapps \
  --set hostname=<domain> \
  --set clientID=<ID> \
  --set clientSecret=<secret> \
  --set cookieSecret=<cookie string> \
  --namespace kubeapps \
  -f kubeapps-values.yaml bitnami/kubeapps
```
