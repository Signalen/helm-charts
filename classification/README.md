# Signalen classification Helm chart

This Helm chart contains the Signalen classification service. It automatically classifies nuisance reports using machine learning.

The classification service expects a trained machine learning model in the folder `/models` on the persistent volume. For more information how to train a model see [the documentation](https://github.com/Signalen/classification/blob/master/README.md).

## Configuration

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `image.repository` | The repository of the Docker image | `signalen/frontend` |
| `image.tag` | The tag of the Docker image | `latest` |
| `signalsCategoryUrl` | This prefix that will be added to every category slug | `http://api.signals.example.com/signals/v1/public/terms` |
| `replicaCount` | The number of API replicas | `1` |
| `ingress.enabled` | Expose the API through an ingress | `true` |
| `ingress.annotations` | Additional annotations on the API ingress | `{}` |
| `ingress.host` | The host of the API | `""` |

Check [values.yaml](./values.yaml) for all the possible configuration options.
