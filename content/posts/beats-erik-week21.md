---
title: "Beats Erik Week21"
date: 2019-03-01T14:43:58-08:00
draft: false
layout: 'posts'
tags: ["Erik Blomquist", "BEATS", "Week 21"]
---

## Monitoring AWS Fargate containers with DataDog
My last post discussed how we add auto scaling by adding targets and policies along with CloudWatch metrics that trigger these policies to either create a new or delete a container. We realized that it is tricky to use CloudWatch along with Fargate and decided that DataDog will be suited for us. 

Fargate allows us to deploy containers with no underlying infrastructure with services and tasks. We can deploy the DataDog Agent to Fargate inside our task definitions so that each of our tasks can be monitored separately. Here is how we do that:


1. Add the following to the task definition of the task that you want to monitor. Replace the the environment variables with your own keys.

        "dockerLabels": {
            "com.datadoghq.ad.instances": "[{\"host\": \"%%host%%\", \"port\": 80}]",
            "com.datadoghq.ad.check_names": "[\"saps-frontend\"]",
            "com.datadoghq.ad.init_configs": "[{}]"
            }
        }, {
            "name": "datadog-agent",
            "image": "datadog/agent:latest",
            "essential": true,
            "environment": [{
                "name": "DD_API_KEY",
                "value": "$YOUR_API_KEY"
            },
            {
                "name": "ECS_FARGATE",
                "value": "true"
            }
            ],
            "requiresCompatibilities": [
            "FARGATE"
            ],
            "cpu": "256",
            "memory": "512"
        }

2. We need to register the new task with our ECS. Open up the AWS cli and run the following command but replace *csun-saps-prod* with your own task definition.

    aws ecs register-task-definition --cli-input-json file://./csun-saps-prod.json

3. Last step is to run the task, so `terraform init` to initialize, `terraform plan` to see what will change, and `terraform apply` to apply the changes. Be aware the it will take some time before the service update the new tasks.





