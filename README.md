# Hands-on Flux2

Demo repository for GitOps with Flux2 using a branch based monorepo per cluster and application repos.

## Prerequisites

You need to have the following tools installed locally to be able to complete all steps:
- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [eksctl](https://eksctl.io/)
- [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [flux](https://fluxcd.io/docs/get-started/)
- [Helm](https://helm.sh/docs/intro/install/)

## Bootstrapping

```bash
# define required ENV variables for the next steps to work
$ export GITHUB_TOKEN=<your-token>
$ export GITHUB_USER=lreimer
$ export AWS_ACCOUNT_ID=export AWS_ACCOUNT_ID=`aws sts get-caller-identity --query Account --output text`

# created 2 branches to track the individual cluster states
$ git branch env/dev
$ git branch env/prod
$ git push --all

# setup the dev cluster and GitOps environment
$ make create-dev-cluster
$ make bootstrap-flux2-dev

# next we start to add things to the cluster
# all files to apply in the env/ branches are in the examples directory

# setup the dev cluster and GitOps environment
$ make create-prod-cluster
$ make bootstrap-flux2-prod

$ make destroy-clusters
```

## Webhook Receivers

```bash
# the relevant files are found in examples/webhook-receiver/
$ git checkout env/dev
$ cp examples/webhook-receiver/ clusters/flux2-dev-cluster/

$ git add -A && git commit -m "Added webhook receiver for flux-system"
$ git push
```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.