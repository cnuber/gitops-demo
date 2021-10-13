# gitops-demo

# Pre-reqs

docker https://www.docker.com/products/docker-desktop
git -- git is likely installed, but if it's not https://github.com/git-guides/install-git

# Install k3d
curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash

# Install kubectl
If you don't already have kubectl installed, visit https://kubernetes.io/docs/tasks/tools/ and follow instructions for your OS

If you are on a Mac and have homebrew, you can install it with `brew install kubectl` or download as intructed in the link above

# Install flux cli v2

Follow instructions at https://fluxcd.io/docs/cmd/

Or if you're feeling froggy: curl -s https://fluxcd.io/install.sh | sh

# Create a k3d cluster

k3d cluster create gitops -p "9999:30001@server[0]"

# Install flux using the flux cli

flux install

# Download the necessary repos for the hands-on lab
mkdir ~/demo-repos # or whever you wanna create a directory for the repos
cd ~/demo-repos # or wherever you created the directory
git clone https://github.com/cnuber/gitops-demo
git clone https://github.com/cnuber/qjs-demo

# Deploy the Flux custom resource objects
cd ~/demo-repos/gitops-demo
kubectl apply -f manifests/qjs-ns.yaml # create the namespace for the demo
kubectl apply -f manifests/qjs-repo-crd.yaml -n qjs # create the repo object 
kubectl apply -f manifests/qjs-kustomization-crd.yaml -n qjs # create the Flux kustomization object to notify flux to deploy the application from the qjs-demo repository and watch for changes

# Verify deployment
kubectl get pods -n qjs # pod should exist and be in a running state 2/2





