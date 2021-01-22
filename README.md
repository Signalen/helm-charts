# Signalen Helm charts

[Signalen](https://signalen.org/) helps cities receive, manage and solve nuisance reports. The software is open source and therefore free for others to use. This repository contains [Helm charts](https://helm.sh/) to easily install the Signalen stack on a Kubernetes cluster.

## External authentication

The Signalen backoffice relies on an external OpenID Connect identity provider for authentication of users. If no instance is available, [Dex](https://github.com/dexidp/dex) could be used with a static user as follows:

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm upgrade --install \
  dex stable/dex \
  --create-namespace \
  --namespace dex \
  --set "config.issuer=https://dex.signalen.example.com" \
  --set "config.staticPasswords[0].email=signals.admin@example.com" \
  --set "config.staticPasswords[0].hash=$2a$10$2b2cU8CPhOTaGrs1HRQuAueS7JTT5ZHsHSzYiFPm1leZck7Mc8T4W" \
  --set "config.staticPasswords[0].username=admin" \
  --set "config.staticClients[0].id=signals" \
  --set "config.staticClients[0].name=Signalen" \
  --set "config.staticClients[0].secret=somethingsecret" \
  --set "config.staticClients[0].redirectURIs[0]=https://signalen.example.com/manage/incidents" \
  --set "config.oauth2.responseTypes={token,id_token}" \
  --set "ingress.enabled=true" \
  --set "ingress.hosts[0]=dex.signalen.example.com"
```

This command will install Dex in the dex namespace and exposes an [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) on dex.signalen.example.com. It creates the following static user:

- E-mail: signalen.admin@example.com
- Password: password

Dex has a lot of benefits when connected to an Identity Provider [using connectors](https://github.com/dexidp/dex#connectors). It provides out-of-the box support for LDAP, SAML2.0, GitHub, GitLab and more.

## Install the charts

First configure the Helm repository:

```bash
helm repo add signalen https://signalen.github.io/helm-charts/
helm repo update
```

Then install the backend chart:

```bash
helm upgrade --install \
  signalen-backend signalen/backend \
  --create-namespace \
  --namespace signalen \
  --set "settings.allowedHosts=api.signals.example.com" \
  --set "settings.defaultPdokMunicipalities=Amsterdam" \
  --set "settings.jwksUrl=https://dex.signals.example.com" \
  --set "settings.userIdField=email" \
  --set "settings.classificationEndpoint=https://classification.signals.example.com/signals_mltool" \
  --set "ingress.enabled=true" \
  --set "ingress.host=api.signals.example.com"
```

And install the frontend chart:

```bash
helm upgrade --install \
  signalen-frontend signalen/frontend \
  --create-namespace \
  --namespace signalen \
  --set "oidc.authEndpoint=https://dex.signals.example.com/auth" \
  --set "config.apiBaseUrl=https://api.signals.example.com/signals" \
  --set "ingress.enabled=true" \
  --set "ingress.host=signals.example.com"
```

And install the classification chart:

```bash
helm upgrade --install \
  signalen-classification signalen/classification \
  --create-namespace \
  --namespace signalen \
  --set "signalsCategoryUrl=https://api.signals.example.com/signals/v1/public/terms" \
  --set "ingress.enabled=true" \
  --set "ingress.host=classification.signals.example.com"
```

## Uninstall the charts

To delete the charts:

```bash
helm delete --namespace signals signals-frontend
helm delete --namespace signals signals-backend
helm delete --namespace signals signals-classification
```

And finally remove the namespace:

```bash
kubectl delete namespace signals
```

## Configuration

Consult the documentation of the specific charts for an overview of all configuration options:

- [backend](./charts/backend)
- [frontend](./charts/frontend)
- [classification](./charts/classification)
