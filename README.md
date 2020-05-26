# Signals

[Signals](https://signalen.org/) helps cities receive, manage and solve nuisance reports. Signalen is open source and therefore free for others to use. This repository contains [Helm charts](https://helm.sh/) to easily install the software.

## Installing the charts

First create the namespace `signals` and initialize the backend chart:

```console
kubectl create namespace signals
(cd signals-backend/ && helm dependency update)
```

Then install the chart:

```console
helm upgrade --install -n signals signals-frontend ./signals-frontend
helm upgrade --install -n signals signals-backend ./signals-backend
```

## Deleting the charts

To delete the charts:

```console
helm delete -n signals signals-frontend
helm delete -n signals signals-backend
```

And finally remove the namespace:

```console
kubectl delete namespace signals
```
