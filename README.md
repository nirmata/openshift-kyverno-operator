# openshift-kyverno-operator
openshift kyverno operator for redhat market place

## For devloper
The following command to initialize new project and build the operator image. To build the operator and bundle image we need operator-sdk. https://sdk.operatorframework.io/docs/installation/ in this link the steps to install operator-sdk.

### Initialize project

```shell
operator-sdk init --plugins=helm --domain=nirmata.io --group=operator --version=v1 --kind=KyvernoOperator --helm-chart=<chart-name> --helm-chart-repo=<chart-repo-url>
```

### To build operator image

```shell
make docker-build docker-push IMG="example.com/openshift-kyverno-operator:v0.0.1"
```

## For devloper and QA
To build the bundle and testing local following command to be use. First need to install olm to the local k8s cluster.

### To install OLM

```shell
operator-sdk olm install
```

### To build bundle image

```shell
make bundle bundle-build bundle-push BUNDLE_IMG="example.com/openshift-kyverno-bundle:v0.0.1" IMG="example.com/openshift-kyverno-operator:v0.0.1"
```

### Run the bundle

```shell
operator-sdk run bundle example.com/openshift-kyverno-bundle:v0.0.1
```

### Apply CR

```shell
kubectl apply -k config/samples
```

https://sdk.operatorframework.io/docs/building-operators/helm/quickstart/ here is the complete document for olm installation, deployment and cleanup.