# Homelab

The map of my DevOps work. Each project is documented from **topology** → **architecture** → **design decisions** → **code** → **production** as it's built.

← [Return to Overview](https://github.com/Buildeployship)

## Service Topology

```
┌──────────────────────────────────────────────────────────────────┐
│ cicd-observability-stack                                         │
│                                                                  │
│                       ┌─────────────────┐  ┌─────────────────┐   │
│                       │    GitLab CE    │  │ Loki · Grafana  │   │
│                       └────────┬────────┘  │  Tempo · Mimir  │   │
│                                v           │  Alloy · OTel   │   │
│                       ┌─────────────────┐  └────────┬────────┘   │
│                       │  GitLab Runner  │           │            │
│                       │ Docker executor │           │            │
│                       └────────┬────────┘           │            │
└────────────────────────────────┼────────────────────┼────────────┘
                                 ┆ runs               ┆ observes
┌────────────────────────────────┼────────────────────┼────────────┐
│ go-cicd-observability          │                    │            │
│                                v                    │            │
│ ┌──────────────────┐  ┌─────────────────┐           │            │
│ │ Go webhook relay │─>│  GitLab CI/CD   │           │            │
│ │       OTel       │  │    7 stages     │           │            │
│ └──────────────────┘  └────────┬────────┘           │            │
│                                v push               v            │
│                       ┌─────────────────┐  ┌─────────────────┐   │
│                       │ GitLab Registry │─>│ Nomad · Consul  │   │
│                       │  image storage  │  │ mTLS · homelab  │   │
│                       └─────────────────┘  │ manual job run  │   │
│                                            └─────────────────┘   │
│                                                                  │
│ ┌──────────────────┐                       ┌─────────────────┐   │
│ │    Terraform     │──────────────────────>│ AWS ECS Fargate │   │
│ │   IaC · manual   │                       │       ALB       │   │
│ └──────────────────┘                       │ deploy · verify │   │
│                                            │    · destroy    │   │
│                                            └─────────────────┘   │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

## Projects

### [cicd-observability-stack](https://github.com/Buildeployship/cicd-observability-stack)
On-premises CI/CD and observability platform, with architecture and operating documentation.

`GitLab CI/CD` `Docker Compose` `LGTM` `Nomad` `Consul` `Tailscale`

### [go-cicd-observability](https://github.com/Buildeployship/go-cicd-observability)
Go webhook relay delivered through a multi-stage pipeline, deployed to on-premises and AWS targets.

`Go` `GitLab CI/CD` `Terraform` `AWS ECS Fargate` `Nomad` `OTel`

### [k8s-terraform-cicd-aws](https://github.com/Buildeployship/k8s-terraform-cicd-aws)
In progress: Kubernetes on AWS, provisioned with Terraform and delivered through CI/CD.

`Kubernetes` `Terraform` `AWS` `GitLab CI/CD`
