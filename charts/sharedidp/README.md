# Helm chart for Shared Keycloak Instance

![Version: 3.0.0](https://img.shields.io/badge/Version-3.0.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 23.0.7](https://img.shields.io/badge/AppVersion-23.0.7-informational?style=flat-square)

This helm chart installs the Helm chart for Shared Keycloak Instance.

For further information please refer to the [technical documentation](../../docs/technical%20documentation).

The referenced container images are for demonstration purposes only.

## Installation

To install the chart with the release name `sharedidp`:

```shell
$ helm repo add tractusx-dev https://eclipse-tractusx.github.io/charts/dev
$ helm install sharedidp tractusx-dev/sharedidp
```

To install the helm chart into your cluster with your values:

```shell
$ helm install -f your-values.yaml sharedidp tractusx-dev/sharedidp
```

To use the helm chart as a dependency:

```yaml
dependencies:
  - name: sharedidp
    repository: https://eclipse-tractusx.github.io/charts/dev
    version: 3.0.0
```

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://raw.githubusercontent.com/bitnami/charts/archive-full-index/bitnami | keycloak | 19.3.0 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| keycloak.auth.adminUser | string | `"admin"` |  |
| keycloak.auth.existingSecret | string | `"sharedidp-keycloak"` | Secret containing the passwords for admin username 'admin' and management username 'manager'. |
| keycloak.production | bool | `false` | Run Keycloak in production mode. TLS configuration is required except when using proxy=edge. |
| keycloak.proxy | string | `"passthrough"` | reverse Proxy mode edge, reencrypt, passthrough or none; ref: https://www.keycloak.org/server/reverseproxy; If your ingress controller has the SSL Termination, you should set proxy to edge. |
| keycloak.httpRelativePath | string | `"/auth/"` | Setting the path relative to '/' for serving resources: as we're migrating from 16.1.1 version which was using the trailing 'auth', we're setting it to '/auth/'. ref: https://www.keycloak.org/migration/migrating-to-quarkus#_default_context_path_changed |
| keycloak.extraEnvVars[0].name | string | `"KEYCLOAK_EXTRA_ARGS"` |  |
| keycloak.extraEnvVars[0].value | string | `"-Dkeycloak.migration.action=import -Dkeycloak.migration.provider=dir -Dkeycloak.migration.dir=/realms -Dkeycloak.migration.strategy=IGNORE_EXISTING"` |  |
| keycloak.replicaCount | int | `3` |  |
| keycloak.extraVolumes[0].name | string | `"themes-catenax-shared"` |  |
| keycloak.extraVolumes[0].emptyDir | object | `{}` |  |
| keycloak.extraVolumes[1].name | string | `"themes-catenax-shared-portal"` |  |
| keycloak.extraVolumes[1].emptyDir | object | `{}` |  |
| keycloak.extraVolumes[2].name | string | `"realms"` |  |
| keycloak.extraVolumes[2].emptyDir | object | `{}` |  |
| keycloak.extraVolumeMounts[0].name | string | `"themes-catenax-shared"` |  |
| keycloak.extraVolumeMounts[0].mountPath | string | `"/opt/bitnami/keycloak/themes/catenax-shared"` |  |
| keycloak.extraVolumeMounts[1].name | string | `"themes-catenax-shared-portal"` |  |
| keycloak.extraVolumeMounts[1].mountPath | string | `"/opt/bitnami/keycloak/themes/catenax-shared-portal"` |  |
| keycloak.extraVolumeMounts[2].name | string | `"realms"` |  |
| keycloak.extraVolumeMounts[2].mountPath | string | `"/realms"` |  |
| keycloak.initContainers[0].name | string | `"import"` |  |
| keycloak.initContainers[0].image | string | `"docker.io/tractusx/portal-iam:v3.0.0"` |  |
| keycloak.initContainers[0].imagePullPolicy | string | `"IfNotPresent"` |  |
| keycloak.initContainers[0].command[0] | string | `"sh"` |  |
| keycloak.initContainers[0].args[0] | string | `"-c"` |  |
| keycloak.initContainers[0].args[1] | string | `"echo \"Copying themes-catenax-shared...\"\ncp -R /import/themes/catenax-shared/* /themes-catenax-shared\necho \"Copying themes-catenax-shared-portal...\"\ncp -R /import/themes/catenax-shared-portal/* /themes-catenax-shared-portal\necho \"Copying realms...\"\ncp -R /import/catenax-shared/realms/* /realms\n"` |  |
| keycloak.initContainers[0].volumeMounts[0].name | string | `"themes-catenax-shared"` |  |
| keycloak.initContainers[0].volumeMounts[0].mountPath | string | `"/themes-catenax-shared"` |  |
| keycloak.initContainers[0].volumeMounts[1].name | string | `"themes-catenax-shared-portal"` |  |
| keycloak.initContainers[0].volumeMounts[1].mountPath | string | `"/themes-catenax-shared-portal"` |  |
| keycloak.initContainers[0].volumeMounts[2].name | string | `"realms"` |  |
| keycloak.initContainers[0].volumeMounts[2].mountPath | string | `"/realms"` |  |
| keycloak.service.sessionAffinity | string | `"ClientIP"` |  |
| keycloak.ingress.enabled | bool | `false` |  |
| keycloak.ingress.ingressClassName | string | `"nginx"` |  |
| keycloak.ingress.hostname | string | `"sharedidp.example.org"` | Provide default path for the ingress record. |
| keycloak.ingress.annotations."cert-manager.io/cluster-issuer" | string | `""` | Enable TLS configuration for the host defined at `ingress.hostname` parameter; TLS certificates will be retrieved from a TLS secret with name: `{{- printf "%s-tls" .Values.ingress.hostname }}`; Provide the name of ClusterIssuer to acquire the certificate required for this Ingress |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/cors-allow-credentials" | string | `"true"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/cors-allow-methods" | string | `"PUT, GET, POST, OPTIONS"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/cors-allow-origin" | string | `"https://sharedidp.example.org"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/enable-cors" | string | `"true"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/proxy-buffer-size" | string | `"128k"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/proxy-buffering" | string | `"on"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/proxy-buffers-number" | string | `"20"` |  |
| keycloak.ingress.annotations."nginx.ingress.kubernetes.io/use-regex" | string | `"true"` |  |
| keycloak.ingress.tls | bool | `true` |  |
| keycloak.rbac.create | bool | `true` |  |
| keycloak.rbac.rules[0].apiGroups[0] | string | `""` |  |
| keycloak.rbac.rules[0].resources[0] | string | `"pods"` |  |
| keycloak.rbac.rules[0].verbs[0] | string | `"get"` |  |
| keycloak.rbac.rules[0].verbs[1] | string | `"list"` |  |
| keycloak.postgresql.enabled | bool | `true` | PostgreSQL chart configuration (recommended for demonstration purposes only); default configurations: host: "sharedidp-postgresql-primary", port: 5432; Switch to enable or disable the PostgreSQL helm chart. |
| keycloak.postgresql.image | object | `{"tag":"15-debian-11"}` | Setting to Postgres version 15 as that is the aligned version, https://eclipse-tractusx.github.io/docs/release/trg-5/trg-5-07/#aligning-dependency-versions). Keycloak helm-chart from Bitnami has moved on to version 16. |
| keycloak.postgresql.commonLabels."app.kubernetes.io/version" | string | `"15"` |  |
| keycloak.postgresql.auth.username | string | `"kcshared"` | Non-root username. |
| keycloak.postgresql.auth.database | string | `"iamsharedidp"` | Database name. |
| keycloak.postgresql.auth.existingSecret | string | `"sharedidp-postgres"` | Secret containing the passwords for root usernames postgres and non-root username kcshared. |
| keycloak.postgresql.architecture | string | `"replication"` |  |
| keycloak.externalDatabase.host | string | `"sharedidp-postgresql-external-db"` | External PostgreSQL configuration IMPORTANT: non-root db user needs needs to be created beforehand on external database. Database host ('-primary' is added as postfix). |
| keycloak.externalDatabase.port | int | `5432` | Database port number. |
| keycloak.externalDatabase.user | string | `"kcshared"` | Non-root username for sharedidp. |
| keycloak.externalDatabase.database | string | `"iamsharedidp"` | Database name. |
| keycloak.externalDatabase.password | string | `""` | Password for the non-root username (default 'kcshared'). Secret-key 'password'. |
| keycloak.externalDatabase.existingSecret | string | `"sharedidp-keycloak-external-db"` | Secret containing the password non-root username, (default 'kcshared'). |
| keycloak.externalDatabase.existingSecretPasswordKey | string | `"password"` | Name of an existing secret key containing the database credentials. |
| secrets.auth.existingSecret.adminpassword | string | `""` | Password for the admin username 'admin'. Secret-key 'admin-password'. |
| secrets.postgresql.auth.existingSecret.postgrespassword | string | `""` | Password for the root username 'postgres'. Secret-key 'postgres-password'. |
| secrets.postgresql.auth.existingSecret.password | string | `""` | Password for the non-root username 'kcshared'. Secret-key 'password'. |
| secrets.postgresql.auth.existingSecret.replicationPassword | string | `""` | Password for the non-root username 'repl_user'. Secret-key 'replication-password'. |
| secrets.realmuser.enabled | bool | `false` |  |

Autogenerated with [helm docs](https://github.com/norwoodj/helm-docs)

## Post-Install Configuration

Once the installation is completed, the following steps need to be executed in the Keycloak admin console:

### Within the master realm

Generate client-secrets for the service account with access type 'confidential'.

### Within the CX-Operator realm

#### Establish connection to the centralidp instance

1. Change the example.org placeholder in the central-idp client the to the address of the centralidp instance:

* Settings --> Valid Redirect URI
* Keys --> JWKS URL

2. Set password and user details for the initial user.

3. Setup SMTP configuration (Realm Settings --> Email)

## Upgrade

### To 3.0.0

This major changes from the Keycloak version from  22.0.3 to 23.0.7 and bumps the PostgresSQL version of the subchart from 15.4.0 to the latest available version of 15.

No major issues are expected during the upgrade.

### To 2.1.0

No specific upgrade notes.

### To 2.0.0

This major changes from the Keycloak version from 16.1.1 to version 22.0.3.

Please have a look at the [CHANGELOG](../../CHANGELOG.md#200) for a more detailed description.

We also recommend checking out the [Keycloak Upgrading Guide](https://www.keycloak.org/docs/latest/upgrading/index.html).

To be explicitly mentioned: this major adds the production mode with default value false and the reverse proxy mode with default value passthrough.
Please check the description of those parameters and decide if they're suitable for you.

#### Upgrade approach

For the overall process of migrating from version 16.1.1 to version 22.0.3., we recommend to follow a blue-green deployment approach. In the following, you find a rough outline of the necessary steps:

1. Scale down current the Keycloak services (blue deployment)
2. Backup the current data
3. Deploy the new Keycloak instance (green deployment e.g: `-green`, `-kc22`, ...) in another namespace than the blue instance
4. Restore the data of the blue instance to the green instance
5. Start the new Keycloak services
6. Once the new/green instance is validated, switch the user traffic to it

#### Upgrade PostgreSQL

Please be aware that this major changes the version of the PostgreSQL subchart by Bitnami from 14.x.x to 15.x.x (subchart updated from version 11.x.x to 12.x.x).

In case you are using an external PostgreSQL instance and would like to upgrade to 15.x, please follow the [official instructions](https://www.postgresql.org/docs/15/upgrading.html).

In case you would like to upgrade the PostgreSQL subchart from Bitnami, we recommend blue-green deployment approach, like described [above](#upgrade-approach).
For restoring the data of the blue instance to the green instance (step 4), execute the following statement using [pg-dumpall](https://www.postgresql.org/docs/current/app-pg-dumpall.html):

On the cluster:

```shell
 kubectl exec -it green-postgresql-primary-0 -n green-namespace -- /opt/bitnami/scripts/postgresql/entrypoint.sh /bin/bash -c 'export PGPASSWORD=""; echo "local all postgres trust" > /opt/bitnami/postgresql/conf/pg_hba.conf; pg_ctl reload; time pg_dumpall -c -h 10-123-45-67.blue-namespace.pod.cluster.local -U postgres | psql -U postgres'
 ```

Or on the primary pod of the new/green PostgreSQL instance:

```shell
/opt/bitnami/scripts/postgresql/entrypoint.sh /bin/bash -c 'export PGPASSWORD=""; echo "local all postgres trust" > /opt/bitnami/postgresql/conf/pg_hba.conf; pg_ctl reload; time pg_dumpall -c -h 10-123-45-67.blue-namespace.pod.cluster.local -U postgres | psql -U postgres'
```

Where '10-123-45-67' is the cluster IP of the old/blue PostgreSQL instance.
