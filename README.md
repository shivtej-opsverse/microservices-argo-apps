# microservices-argo-apps

Defines a set of Argo CD `Application` resources to deploy a cloud-native microservices demo application known as Online Boutique from GCP at https://github.com/GoogleCloudPlatform/microservices-demo

## Purpose

To allow a single repo for Argo CD to easily deploy a sample application for demonstration of logs, metrics, and traces coming into [DevOpsNow's ObserveNow backend](https://www.devopsnow.io) - a fully-managed observability stack comprising of open-source tools such as Jaeger, Grafana, Loki, Prometheus, and VictoriaMetrics.

## Assumptions

* You already have access to your own Kubernetes cluster (or a managed one like EKS, GKE, AKS, etc)
* You have an Argo CD setup for which declarative, GitOps-based continuous deployment lifecycle is managed (if you don't have access to this, you may request one [from DevOpsNow via its DeployNow](https://www.devopsnow.io) managed Argo CD service). 

## Usage

Via Argo CD UI or CLI (shown here), you can:

1.  Log into your Argo instance:

    ```
    $ argocd login ${ARGO_HOST} --username ${ARGO_USER} --password ${ARGO_PASS} --grpc-web
    ```

2.  Fork this Git repo and add it:

    ```
    $ argocd repo add https://github.com/<yourFork>/microservices-argo-apps
    ```

3.  Add your target Kubernetes cluster (ensure you're on it):

    ```
    $ argocd cluster add <nameOfCluserConfig> --name <nameOfCluster>
    $ argocd cluster list
    ```

4.  Once added, **make sure** `spec.source.repoURL` and `spec.destination.server` point to their respective names from (2) and (3) above

5.  Add the project to point to `/apps` so future changes get automatically deployed:

   ```
   $ argocd app create apps-of-apps --repo https://repo/from/step/2 --path apps --revision HEAD --dest-server https://<fromStep3> --directory-recurse
   ```
