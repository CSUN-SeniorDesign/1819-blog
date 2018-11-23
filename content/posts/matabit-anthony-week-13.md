---
title: "Anthony Week 13"
date: 2018-11-23T11:42:09-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 13"]
layout: 'posts'
---
# Week 13
This was a short week due to Thanksgiving so I worked on continuing  the base layer of the infrastructure so I don't have to worry about it over break. I've met with the Devs Monday so we can wireframe the design of the web app. We also wrote sudo code the the various controllers and middleware for the project. 

## AWS
On the AWS side, I created a lightsail instance to host drone, the CICD tool. I choose lightsail because it'll be cheaper to host, plus I don’t really need all the fancy features/configurations a EC2 come with. A T.2 Micro would cost ~$8 a month compared to similarly configure lightsail instance for $5. 

```json
resource "aws_lightsail_instance" "matabit_drone" {
  name              = "matabit-drone"
  availability_zone = "us-west-2a"
  blueprint_id      = "ubuntu_18_04"
  bundle_id         = "nano_2_0"
  key_pair_name     = "anthony"
}
```

I also created a RDS instance and opted to use a module. This allows me to reuse the same resource blocks without having to copy and paste the same configs for every new instance. 

```json
module "rds-dev" {
  source         = "../modules/rds"
  rds-name       = "matabit"
  user           = "dba_matabit"
  password       = "${var.password}"
  instance-class = "db.t2.micro"
  final-snapshot = "matabit-final-snapshot"
  port-number    = "3306"
  multi-zone     = "false"
  subnet-name    = "${aws_db_subnet_group.default.name}"
  security-group = "${aws_security_group.default.id}"
  public-access  = "true"
}
```

## Drone
The team was almost ready to develop so I created the Github Repo in the COMP490 organization. I linked the Org to Drone and configured the CI pipeline to run test. I haven't started the deployment part yet as there's nothing to deploy. Since Drone runs in containers each pipeline uses an image. I have two "steps" one to install composer dependancies and another to run phpunit. 

```yml
---
kind: pipeline
name: default

platform:
  os: linux
  arch: amd64

steps:
- name: Install composer
  image: composer
  commands:
  - composer install --prefer-dist

- name: PHPUnit Test
  image: php:7.3-rc-cli-alpine
  commands:
  - php -v
  - cp .env.example .env
  - php artisan key:generate
  - vendor/bin/phpunit --configuration phpunit.xml

...
```


