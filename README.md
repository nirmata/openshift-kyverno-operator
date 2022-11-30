# openshift-kyverno-operator
openshift kyverno operator for redhat market place

## For developer
The following command to initialize new project and build the operator image. To build the operator and bundle image we need operator-sdk. https://sdk.operatorframework.io/docs/installation/ in this link the steps to install operator-sdk.

### Initialize project

```shell
operator-sdk init --plugins=helm --domain=nirmata.io --group=operator --version=v1 --kind=KyvernoOperator --helm-chart=<chart-name> --helm-chart-repo=<chart-repo-url>
```

### To build operator image

```shell
make docker-build docker-push IMG="ghcr.io/nirmata/openshift-kyverno-operator:v0.0.1"
```

## For developer and QA
To build the bundle and testing local following command to be use. First need to install olm to the local k8s cluster.

### To install OLM

```shell
operator-sdk olm install
```

### To build bundle image

```shell
make bundle bundle-build bundle-push BUNDLE_IMG="ghcr.io/nirmata/openshift-kyverno-bundle:v0.0.1" IMG="ghcr.io/nirmata/openshift-kyverno-operator:v0.0.1"
```

### Run the bundle

```shell
operator-sdk run bundle ghcr.io/nirmata/openshift-kyverno-bundle:v0.0.1
```

### Apply CR

The container images used in the internal Kyverno and Kyverno Operator pods are private. Get necessary user/password from Nirmata, and set these in the file `config/samples/operator_v1_openshiftkyvernooperator.yaml ` under the locations:
- image.pullSecrets.username, image.pullSecrets.password
- imagePullSecret.username, imagePullSecret.password

Then create the OpenshiftKyvernoOperator custom resource by running

```shell
kubectl apply -k config/samples
```
The Kyverno and Kyverno-Operator pods should show as running.

https://sdk.operatorframework.io/docs/building-operators/helm/quickstart/ here is the complete document for olm installation, deployment and cleanup.