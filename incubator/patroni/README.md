# Patroni Helm Chart

This chart deploys a [Patroni](https://github.com/zalando/patroni/) cluster using a StatefulSet. It is based on 
an image which allows Patroni to be run without an external DCS (Consul, Etcd, Zookeeper).

See https://github.com/zalando/spilo/pull/198 and https://github.com/zalando/patroni/pull/500.

## Prerequisites Details

* Kubernetes 1.7+
* PV support on the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
$ helm dependency update
$ helm install --name my-release incubator/patroni-k8s
```

## Configuration

The following tables lists the configurable parameters of the patroni-k8s chart and their default values.

| Parameter | Description | Default |
|-------------------------|-------------------------------------|-----------------------------------------------------|
| `image.repository` | Patroni image repository | `unguiculus/patroni` |
| `image.tag` | Patroni image tag | `20171122.132733-3f08711` |
| `image.pullPolicy` | Patroni image pull policy | `IfNotPresent` |
| `replicas` | number of statefulset replicas | `3` |
| `nodeSelector` | nodeSelector map | `{}` |
| `tolerations` | node taints to tolerate  | `[]` |
| `resources` | pod resource requests and limits | `{}` |
| `credentials.superuser` | password for the superuser | `tea` |
| `credentials.replication` | password for the replication user | `pinacolada` |
| `walE.enable`           | use of wal-e tool for base backup/restore | `false` |
| `walE.scheduleCronJob` | schedule of wal-e backups          | `00 01 * * *` |
| `walE.retainBackups`   | number of backups to retain         | `2` |
| `walE.s3Bucket:`       | Amazon S3 bucket used for wal-e backups | `` |
| `walE.gcsBucket`       | Google cloud plataform storage used for wal-e backups | `` |
| `walE.kubernetesSecret` | kubernetes secret for provider bucket | `` |
| `walE.backupThresholdMegabytes` | maximum size of the WAL segments accumulated after the base backup to consider WAL-E restore instead of pg_basebackup | `1024` |
| `walE.backupThresholdPercentage` | maximum ratio (in percents) of the accumulated WAL files to the base backup to consider WAL-E restore instead of pg_basebackup | `30` |
| `persistentVolume.enabled` | specifies whether persistent volumes are used | `true` |
| `persistentVolume.accessModes` | persistent volume access modes | `[ReadWriteOnce]` |
| `persistentVolume.annotations` | annotations for persistent volume claim` | `{}` |
| `persistentVolume.size` | persistent volume size | `1Gi` |
| `persistentVolume.storageClass` | persistent volume storage class | `volume.alpha.kubernetes.io/storage-class: default` |
| `cleanup.image.repository` | cleanup image repository | `quay.io/coreos/hyperkube` |
| `cleanup.image.tag` | cleanup image tag | `v1.8.4_coreos.0` |
| `cleanup.image.pullPolicy` | cleanup image pull policy | `IfNotPresent` |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml incubator/patroni-k8s
```

> **Tip**: You can use the default [values.yaml](values.yaml)
