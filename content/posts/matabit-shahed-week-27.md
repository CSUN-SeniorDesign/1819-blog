---
title: "Matabit Shahed Week 27"
date: 2019-04-19T19:05:43-07:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 27"]
layout: "posts"
---
# Troubleshooting Containers...

Our task definitions consist of two containers, which both run at 256MB RAM. This means that a single task definition will take up 512MB of RAM. Since we run our ECS tasks on a single t2.micro instance, we don't have enough RAM to run two tasks. Especially, since we would have to run 4 task definitions in parallel to update the services. 

To accomodate for everything, we changed the task definition so that each container only takes up 128MB of RAM and changed the desired count to 1 so that we can at least run two task definitions at the same time.

```JSON
[
    {
        "name": "frontend",
        "image": "485876055632.dkr.ecr.us-west-2.amazonaws.com/csun-saps:frontend",
        "essential": true,
        "portMappings": [{
            "containerPort": 80,
            "hostPort": 0,
            "protocol": "tcp"
        }],
        "cpu": 128,
        "memory": 128
    },
    {
        "name": "backend",
        "image": "485876055632.dkr.ecr.us-west-2.amazonaws.com/csun-saps:backend",
        "essential": true,
        "cpu": 128,
        "memory": 128
    }
]
```

```JS
module "app-service" {
  source              = "../../modules/ecs"

  name                = "frontend"

  environment         = "prod"
  
  private_subnet_ids  = "${data.terraform_remote_state.vpc.private_subnets}"
  target_group_id     = "${data.terraform_remote_state.alb.alb_target_group_id}"
  cluster_id          = "${data.terraform_remote_state.ecs_cluster.cluster_id}"
  cluster_name        = "${data.terraform_remote_state.ecs_cluster.cluster_name}"
  vpc_id              = "${data.terraform_remote_state.vpc.vpc_id}"
  port                = 80
  desired_count       = 1
}
```

# Troubleshooting the CI/CD Pipeline

Our builds were suddenly starting to fail, which was due to a wrong service name.
The service name was set to "app", even though the actual service was changed to "frontend".
After changing the service name in the command that updates the services, everything ran sucessfully again.

```YML
deploy-prod:
  stage: deploy-prod
  image: docker:latest
  script:
    - apk add --no-cache curl jq python py-pip
    - pip install awscli
    - aws ecs update-service --cluster saps-prod-cluster --service frontend --force-new-deployment
  only:
    refs:
      - master
```


