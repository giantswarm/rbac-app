[![CircleCI](https://circleci.com/gh/giantswarm/rbac-bootstrap-app.svg?style=shield)](https://circleci.com/gh/giantswarm/rbac-bootstrap-app)

# RBAC bootstrap chart

Giant Swarm offers a RBAC bootstrap App as a configuration package to bootstrap the initial role bindings.
Here we define the rbac chart with its templates and default configuration.

**What is this app?**

- You can configure the list of users and groups that will have access to the cluster initially.

**Why did we add it?**

- Some customers want to define a set of bindings as a base to every cluster in order to enable their automation and admin to get quick access to the workload cluster API.

**Who can use it?**

- This app run in all clusters and versions.

## Installing

There are 3 ways to install this app onto a workload cluster.

1. [Using our web interface](https://docs.giantswarm.io/ui-api/web/app-platform/#installing-an-app)
2. [Using our API](https://docs.giantswarm.io/api/#operation/createClusterAppV5)
3. Directly creating the [App custom resource](https://docs.giantswarm.io/ui-api/management-api/crd/apps.application.giantswarm.io/) on the management cluster.

## Configuring

### values.yaml
**This is an example of a values file you could upload using our web interface.**
```
# values.yaml
bindings:
  - role: edit
    users:
      - editor1@myco.com
      - editor2@myco.com
    groups:
      - devops
    namespaces:
      - ns1
      - ns2
  - role: admin
    users:
      - admin@myco.com
    groups:
      - adminteam
```

This configuration create role bindings for `editor1@myco.com` and `editor1@myco.com` in the `ns1` and `ns2` namespaces. At the same time it creates a cluster role binding for `admin@myco.com` and `adminteam` to `admin` cluster role.

### Sample App CR and ConfigMap for the management cluster
If you have access to the Kubernetes API on the management cluster, you could create
the App CR and ConfigMap directly.

Here is an example that would install the app to
workload cluster `abc12`:

```
# appCR.yaml

```

```
# user-values-configmap.yaml
bindings:
  - role: edit
    users:
      - editor1@myco.com
      - editor2@myco.com
    groups:
      - devops
    namespaces:
      - ns1
      - ns2
  - role: admin
    users:
      - admin@myco.com
    groups:
      - adminteam
```

See our [full reference page on how to configure applications](https://docs.giantswarm.io/app-platform/app-configuration/) for more details.

## Compatibility

This app has been tested to work in all the clusters and providers.

## Limitations

This app does not create any role or cluster role, just bind built-in cluster roles to a list of users and groups.