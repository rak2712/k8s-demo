# Jenkins + Kubernetes Demo

This demo shows how to use Jenkins to build, test, dockerize, and deploy a Node.js app to Kubernetes.

## Steps to run

1. Clone this repo.
2. Setup Docker registry credentials in Jenkins (credential id `docker-hub-cred`).
3. Setup Kubernetes config / kubeconfig as a secret file credential in Jenkins (credential id `kubeconfig-cred`).
4. Configure your Kubernetes cluster (minikube, kind, or cloud cluster).
5. Create a Pipeline job pointing at this repo.
6. Push commits; Jenkins should run pipeline automatically.
