# Apache Superset

Apache Superset component installs Apache Superset tool which provides a portal for business intelligence. It provides tools for exploring and visualizing datasets and creating business intelligence dashboards. Superset can also connect to SQL databases for data access. For more information please visit [Apache Superset](https://superset.incubator.apache.org/)  

### Folders
There is one main folder in the Superset component
1. base: contains all the necessary yaml files to install Superset

### Installation
To install Superset add the following to the `kfctl` yaml file.

```
  - kustomizeConfig:
      repoRef:
        name: manifests
        path: superset
    name: superset
```

By default the user and password to the Superset portal is admin/admin. To launch the portal, go to the routes in the namespace you installed Open Data Hub and click on the route with `superset` name.

### Parameters

There are 11 parameters exposed via KfDef.

#### storage_class

Name of the storage class to be used for PVC created by Hue's database. This requires `storage-class` **overlay** to be enabled as well to work.
#### superset_secret

The secret containing the environment variables to set the admin credentials and the Superset app secret key used to encrypt information in the database. If not specified, environment variables from [`superset`](base/secret-superset.yaml) will be used.

When creating a custom secret, the following information should be added:

* **SUPERSET_ADMIN_USER**: The username of the Superset administrator. If using the `superset` secret, `admin` will be used
* **SUPERSET_ADMIN_FNAME**: The First Name of the Superset administrator. If using the `superset` secret, `admin` will be used
* **SUPERSET_ADMIN_LNAME**: The LastName of the Superset administrator. If using the `superset` secret, `admin` will be used
* **SUPERSET_ADMIN_EMAIL**: The e-mail of the Superset administrator. If using the `superset` secret, `admin@fab.org` will be used
* **SUPERSET_ADMIN_PASSWORD**: The password of the Superset administrator. If using the `superset` secret, `admin` will be used
* **SUPERSET_SECRET_KEY**: The app secret key used by Superset to encrypt information in the database. If using the `superset` secret, `thisISaSECRET_1234` will be used

#### superset_db_secret

This parameter configures the Superset database. The secret of choice must contain `database-name`, `database-user`, and `database-password` keys. If not set, credentials from [`supersetdb-secret`](base/supersetdb-secret.yaml) will be used instead.

#### superset_memory_requests

This parameter will configure the Memory request for Superset. If not set, the default value `1Gi` will be used instead.

#### superset_memory_limits

This parameter will configure the Memory limits for Superset. If not set, the default value `2Gi` will be used instead.

#### superset_cpu_requests

This parameter will configure the CPU request for Superset. If not set, the default value `300m` will be used instead.

#### superset_cpu_limits

This parameter will configure the CPU limits for Superset. If not set, the default value `2` will be used instead.

#### superset_db_memory_requests

This parameter will configure the Memory request for Superset Database. If not set, the default value `300Mi` will be used instead.

#### superset_db_memory_limits

This parameter will configure the Memory limits for Superset Database. If not set, the default value `1Gi` will be used instead.

#### superset_db_cpu_requests

This parameter will configure the CPU request for Superset Database. If not set, the default value `300m` will be used instead.

#### superset_db_cpu_limits

This parameter will configure the CPU request for Superset Database. If not set, the default value `1` will be used instead.

### Superset config file customization

Superset manifests comes with a [`superset-config`](base/secret.yaml) secret, which will configure basic parameters for Superset like the database SQLAlchemy URL to connect to its database.

### Superset Database Initialization

Prior to running, Superset's database must be initialized. This is handled via the `superset-init` initContainer. Once this is done, the Superset pod should
start running without intervention. If the database is already initialized the initContainer just checks if everything is as expected and finishes with success.
