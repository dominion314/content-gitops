## Use this repo to run flux and watch it deploy a K8S change. 

https://github.com/dominion314/content-gitops

## Download fluxctl

sudo wget https://github.com/fluxcd/flux/releases/download/1.25.4/fluxctl_linux_amd64 -O /usr/bin/fluxctl && sudo chmod +x /usr/bin/fluxctl

## Export github handle

export GHUSER=[dominion314]

## Create flux namespace

kubectl create ns flux

## This is the command needed to connect flux to github.

  fluxctl install \
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/content-gitops.git \
--git-path=namespaces,workloads \
--namespace=flux | kubectl apply -f -

## Deploy flux and connect it to your github

fluxctl install \
--git-user=${GHUSER} \
--git-email=${GHUSER}@users.noreply.github.com \
--git-url=git@github.com:${GHUSER}/content-gitops \
--git-path=namespaces,workloads \
--namespace=flux | kubectl apply -f -

## Verify the deployment

kubectl -n flux rollout status deployment/flux

## Get the RSA key. You will use this in the repo settings as a deploy key

fluxctl identity --k8s-fwd-ns flux

## This will sync your repo with flux. Flux will do a pull of the repo and add any objects that dont yet exist in your cluster. This is the flux magic.

fluxctl sync --k8s-fwd-ns flux

## Verify the new pod and namespace were added

 kubectl get namespaces
 kubectl get pod --all

