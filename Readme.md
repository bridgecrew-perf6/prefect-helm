# Prefect Helm Charts



## Usage

### TL;DR
``` bash
$ helm repo add prefect https://prefecthq.github.io/prefect-helm
$ helm search repo prefect
$ helm install my-release prefecthq/<chart>


### Installing released versions

Charts are manually versioned depending on chart changes.

The charts are hosted in a [Helm repository](https://helm.sh/docs/chart_repository/) deployed to the web courtesy of Github Pages.

1. Let you local Helm manager know about the repository.

    ```
    $ helm repo add prefecthq https://prefecthq.github.io/prefect-helm/
    ```

2. Sync versions available in the repo to your local cache.

    ```
    $ helm repo update
    ```

3. Search for available charts and versions

    ```
    $ helm search repo [name]
    ```

4. Install the Helm chart

    Using default options
    ```
    $ helm install prefecthq/prefect-orion --generate-name
    ```

    Setting some typical flags for customization
    ```shell
    # The kubernetes namespace to install into, can be anything or excluded to install in the default namespace
    NAMESPACE=prefect-orion
    # The Helm "release" name, can be anything but we recommend matching the chart name
    NAME=prefect-orion
    # The path to your config that overrides values in `values.yaml`
    CONFIG_PATH=path/to/your/config.yaml
    # The chart version to install
    VERSION=2022.06.30

    helm install \
        --namespace $NAMESPACE \
        --version $VERSION \
        --values $CONFIG_PATH \
        $NAME \
        prefecthq/prefect-server
    ```

    _If chart installation fails, `--debug` can provide more information_

    See [Helm install docs](https://helm.sh/docs/helm/helm_install/) for all options.

### Installing development versions

Development versions of the Helm chart will always be available directly from this Github repository.

1. Clone repository

2. Change to this directory

3. Download the postgresql dependency if you are not using an existing database

    ```
    $ helm dependency update
    ```

4. Install the chart

    ```
    $ helm install . --generate-name
    ```

### Upgrading

1. Look up the name of the last release

    ```
    $ helm list
    ```

2. Run the upgrade

    ```shell
    # Set this name to the name of your last Helm release
    NAME=prefect-orion
    # Choose a version to upgrade to or omit the flag to use the latest version
    VERSION=2022.06.30

    helm upgrade $NAME prefecthq/prefect-orion --version $VERSION
    ```

    For development versions, make sure your cloned repository is updated (`git pull`) and reference the local chart
    ```
    $ helm upgrade $NAME .
    ```

3. Upgrades can also be used enable features or change options

    ```shell
    NAME=prefect-orion

    helm upgrade \
        $NAME \
        prefecthq/prefect-orion
    ```

#### Important notes about upgrading

- Updates will only update infrastructure that is modified.
- You will need to continue to set any values that you set during the original install (e.g. `--set agent.enabled=true` or `--values path/to/config.yaml`).
- If you are using the postgresql subchart with an autogenerated password, it will complain that you have not provided that password for the upgrade.
  Export the password as the error asks then set it within the subchart using
    ```
    $ helm upgrade ... --set postgresql.postgresqlPassword=$POSTGRESQL_PASSWORD
    ```

## Options:

See comments in `values.yaml`.
