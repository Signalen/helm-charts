# Signalen backend Helm chart

The backend Helm chart installs the Signalen API and the by default the following dependencies using subcharts:

- PostgreSQL
- RabbitMQ
- Elasticsearch

## Configuration

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `tags.postgresql` | Install PostgreSQL subchart | `true` |
| `tags.rabbitmq` | Install RabbitMQ subchart | `true` |
| `tags.elasticsearch` | Install Elasticsearch subchart | `true` |
| `image.repository` | The repository of the Docker image | `signalen/frontend` |
| `image.tag` | The tag of the Docker image | `latest` |
| `replicaCount` | The number of API replicas | `1` |
| `workerReplicaCount` | The number of background worker replicas | `1` |
| `settings.allowedHosts` | Restrict the allowed hosts of the API | `*` |
| `settings.defaultPdokMunicipalities` | A (comma-seperated) list of [PDOK municipalities](https://www.pdok.nl/introductie/-/article/cbs-wijken-en-buurten) the API allows complaints for (e.g. `"Amsterdam,'s-Hertogenbosch"`) | `""` |
| `settings.jwksUrl` | The JWKS url of the OpenID Connect Identity Provider | `""` |
| `settings.userIdField` | The JWKS url of the OpenID Connect Identity Provider | `""` |
| `settings.email.hostname` | The hostname of the SMTP server for e-mail notifications | `""` |
| `settings.email.port` | The port of the SMTP server | `""` |
| `settings.email.useTLS` | Use TLS while connecting to the SMTP server | `false` |
| `settings.email.useSSL` | Use SSL while connecting to the SMTP server | `false` |
| `settings.classificationService` | Use SSL while connecting to the SMTP server | `https://api.data.amsterdam.nl/signals_mltool` |
| `sigmax.enabled` | Enable the connection with Sigmax City Control | `false` |
| `sigmax.serverUrl` | The server URL of Sigmax | `` |
| `sigmax.authToken` | The token to authenticate with Sigmax | `` |
| `ingress.enabled` | Expose the API through an ingress | `true` |
| `ingress.annotations` | Additional annotations on the API ingress | `{}` |
| `ingress.host` | The host of the API | `""` |

Check [values.yaml](./values.yaml) for all the possible configuration options.
