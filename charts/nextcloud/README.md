#  Nextcloud

![Version: 0.1.4](https://img.shields.io/badge/Version-0.1.4-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 20.0.1-apache](https://img.shields.io/badge/AppVersion-20.0.1-informational?style=flat-square)

A Helm chart for Nextcloud on Kubernetes

## TL;DR

```bash
$ helm repo add groundhog2k https://groundhog2k.github.io/helm-charts/
$ helm install my-release groundhog2k/nextcloud
```

## Introduction

This chart uses the original [Nextcloud from Docker](https://hub.docker.com/_/nextcloud) to deploy Nextcloud in Kubernetes.

It allows fully supports the deployment of the [ARM64v8 image of Nextcloud](https://hub.docker.com/r/arm64v8/nextcloud) on a ARM64 based Kubernetes cluster just by exchanging the existing `image.repository` value.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.x
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release groundhog2k/nextcloud
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm uninstall my-release
```

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| @groundhog2k | mariadb | 0.1.3 |
| @groundhog2k | postgres | 0.1.1 |
| @groundhog2k | redis | 0.1.1 |

## Common parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| fullnameOverride | string | `""` | Fully override the deployment name |
| nameOverride | string | `""` | Partially override the deployment name |

## Deployment parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"nextcloud"` | Image name |
| image.tag | string | `""` | Image tag |
| imagePullSecrets | list | `[]` | Image pull secrets |
| livenessProbe | object | `see values.yaml` | Liveness probe configuration |
| startupProbe | object | `see values.yaml` | Startup probe configuration |
| resources | object | `{}` | Resource limits and requests |
| nodeSelector."kubernetes.io/arch" | string | `"amd64"` | Deployment node selector |
| podAnnotations | object | `{}` | Additional pod annotations |
| podSecurityContext | object | `see values.yaml` | Pod security context |
| securityContext | object | `see values.yaml` | Container security context |
| env | list | `[]` | Additional container environmment variables |
| serviceAccount.create | bool | `false` | Enable service account creation |
| serviceAccount.name | string | `""` | Optional name of the service account |
| serviceAccount.annotations | object | `{}` | Additional service account annotations |
| affinity | object | `{}` | Affinity for pod assignment |
| tolerations | list | `[]` | Tolerations for pod assignment |
| containerPort | int | `8000` | Internal http container port |
| replicaCount | int | `1` | Number of replicas |
| initImage.pullPolicy | string | `"IfNotPresent"` | Init container image pull policy |
| initImage.repository | string | `"busybox"` | Default init container image |
| initImage.tag | string | `"latest"` | Init container image tag |

## Cron job

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| cronJobImage.pullPolicy | string | `"IfNotPresent"` | Cron job container image pull policy|
| cronJobImage.repository | string | `"busybox"` | Default cron job container image |
| cronJobImage.tag | string | `"latest"` | Cron job container image tag |

## Service paramters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.port | int | `80` | Commento HTTP service port |
| service.type | string | `"ClusterIP"` | Service type |

## Ingress parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ingress.enabled | bool | `false` | Enable ingress for Nextcloud service |
| ingress.annotations | string | `nil` | Additional annotations for ingress |
| ingress.hosts[0].host | string | `""` | Hostname for the ingress endpoint |
| ingress.tls | list | `[]` | Ingress TLS parameters |
| ingress.maxBodySize | string | `"64m"` | Maximum body size for post requests |

## Redis session cache

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| externalCache.enabled | bool | `false` | Enable external Redis cache |
| externalCache.host | string | `nil` | External Redis host |
| externalCache.password | string | `nil` | External Redis password |
| externalCache.port | int | `6379` | External Redis port |
| redis.enabled | bool | `false` | Enable Redis cache deployment (will disable external cache settings) |
| redis.storage | string | `nil` | Redis storage settings |

## Database settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| externalDatabase.host | string | `nil` | External database host |
| externalDatabase.name | string | `"nextcloud"` | External database name |
| externalDatabase.user | string | `nil` | External database user name |
| externalDatabase.password | string | `nil` | External database user password |
| externalDatabase.type | string | `"sqlite"` | External database type (mariadb/mysql or postgres - default: sqlite) |
| mariadb.enabled | bool | `false` | Enable MariaDB deployment (will disable external database settings) |
| mariadb.settings.arguments[0] | string | `"--character-set-server=utf8mb4"` | Enable MariaDB UTF8MB4 character set|
| mariadb.settings.arguments[1] | string | `"--collation-server=utf8mb4_unicode_ci"` | Enable UTF8MB4 unicode |
| mariadb.settings.rootPassword | string | `nil` | MariaDB root user password |
| mariadb.storage | string | `nil` | MariaDB storage settings |
| mariadb.userDatabase.name | string | `nil` | MariaDB nextcloud database name |
| mariadb.userDatabase.password | string | `nil` | MariaDB nextcloud database user |
| mariadb.userDatabase.user | string | `nil` | MariaDB nextcloud database user password |
| postgres.enabled | bool | `false` | Enable PostgreSQL deployment (will disable external database settings) |
| postgres.settings.superuserPassword | string | `nil` | PostgreSQL superuser password |
| postgres.storage | string | `nil` | PostgreSQL storage settings |
| postgres.userDatabase.name | string | `nil` | PostgreSQL nextcloud database name |
| postgres.userDatabase.user | string | `nil` | PostgreSQL nextcloud database user |
| postgres.userDatabase.password | string | `nil` | PostgreSQL nextcloud database user password |

## Nextcloud parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| apacheDefaultSiteConfig | string | `""` | Overwrite default apache 000-default.conf |
| apachePortsConfig | string | `""` | Overwrite default apache ports.conf |
| customPhpConfig | string | `""` | Additional PHP custom.ini |
| settings.admin.name | string | `nil` | Nextcloud administrator user |
| settings.admin.password | string | `nil` | Nextcloud admin user password |
| settings.update | bool | `false` | Enable update (set `true` when upgrading nextcloud version with `helm upgrade`) |
| settings.databaseUpdateDelay | int | `30` | Delay for database update after nextcloud upgrade |
| settings.maxFileUploadSize | string | `64M` | Maximum file upload size |
| settings.disableRewriteIP | bool | `false` | Disable rewriting IP address |
| settings.trustedDomains | string | `""` | List of trusted domains separated by blank space |
| settings.trustedProxies | string | `"10.0.0.0/8"` | Trusted proxies |
| settings.smtp.enabled | bool | `false` | Enable SMTP |
| settings.smtp.authType | string | `"LOGIN"` | SMTP auth type (default: LOGIN) |
| settings.smtp.domain | string | `nil` | SMTP domain |
| settings.smtp.from | string | `nil` | SMTP from address |
| settings.smtp.host | string | `nil` | SMTP host |
| settings.smtp.port | int | `465` | SMTP port |
| settings.smtp.name | string | `nil` | SMTP user name |
| settings.smtp.password | string | `nil` | SMTP password |
| settings.smtp.secure | bool | `true` | Use secure SMTP |

## Storage parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| storage.nextcloud | object | `{}` | Nextcloud internal storage |
| storage.nextcloud.accessModes[0] | string | `"ReadWriteOnce"` | Storage access mode |
| storage.nextcloud.persistentVolumeClaimName | string | `""` | PVC name when existing storage volume should be used |
| storage.nextcloud.requestedSize | string | `""` | Size for new PVC, when no existing PVC is used |
| storage.nextcloud.className | string | `""` | Storage class name |
| storage.nextcloudData | object | `{}` | Nextcloud user data storage |
| storage.nextcloudData.accessModes[0] | string | `"ReadWriteOnce"` | Storage access mode |
| storage.nextcloudData.persistentVolumeClaimName | string | `""` | PVC name when existing storage volume should be used |
| storage.nextcloudData.requestedSize | string | `""` | Size for new PVC, when no existing PVC is used |
| storage.nextcloudData.className | string | `""` | Storage class name |