# kubernetes-prometheus

## Background

This **Prometheus** set up has a very specific use case. This is a combination
of
[various](https://coreos.com/blog/prometheus-and-kubernetes-up-and-running.html)
[other](https://github.com/prometheus/prometheus/blob/master/documentation/examples/prometheus-kubernetes.yml)
[sources](https://github.com/kubernetes/kops/blob/master/vendor/github.com/google/cadvisor/docs/storage/prometheus.md).
If you want to create a prometheus in a separate namespace with RBAC control, then this is for you!

## Usage

**Currently, this set up  works with Kubernetes v1.7.1, at the time of writing
v1.8.0 has not been released for Kops**

### Run

From the root directory, apply the following:

  ```bash
  kubectl apply -f namespace.yml
  kubectl apply -f configmap.yml
  kubectl apply -f roles.yml
  kubectl apply -f deployment.yml
  kubectl apply -f service.yml
  ```
A _service_ is configured but not exposed to the public. This service a dummy template,
you can either attach it to a **AWS Elastic Load Balancer**
or expose a **Node Port**.

To access Prometheus, we are going to port forward your localhost port to the **pod**
directly:

```bash
kubectl get pods -n prometheus-dev | awk '{ if (NR!=1) {print "kubectl port-forward " $1 " -n prometheus-dev 9090:9090"} }' | bash
```

> Ensure that port 9090 on localhost is not being used by another service

In your web browser enter:

`http://localhost:9090`

This should redirect you to the **Prometheus** page.
