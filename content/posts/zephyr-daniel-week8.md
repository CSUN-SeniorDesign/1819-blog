---
title: "Project 3 Post 2 - Daniel"
author: "Daniel Hirahoka"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 8"]
date: 2018-10-19T19:29:57-07:00
draft: false
---
This week I was finishing up the Dockerfile to be used in conjunction with the CircleCI. Last week we ran into an error with the Dockerfile where it was failing to find the theme directory. This error caused the container to fail when building and it would stop at this point.

This is our Dockerfile as of now:

~~~
FROM alpine:3.5 as build

ENV HUGO_VERSION 0.49.1
ENV HUGO_BINARY hugo_${HUGO_VERSION}_Linux-64bit.tar.gz

# Install Hugo
RUN set -x && \
  apk add --update wget ca-certificates && \
  wget https://github.com/spf13/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY} && \
  tar xzf ${HUGO_BINARY} && \
  rm -r ${HUGO_BINARY} && \
  mv hugo /usr/bin && \
  apk del wget ca-certificates && \
  rm /var/cache/apk/*

COPY zephyr /site/

WORKDIR /site

RUN /usr/bin/hugo

FROM nginx:alpine

COPY default.conf /etc/nginx/conf.d/default.conf

COPY --from=build /site/public /var/www/site

WORKDIR /var/www/site

RUN ls -al 
~~~

Not much has changed from the previous Dockerfile except for the line where we copy the contents of our blog. This is now referencing the final directory location that is in our Github instead of our local testing environment. Which I think also was the issue with the Theme error. I think it was a syntax error where it was able to find the theme directory because it was correctly copying over the files. 

Afterwards we had to finish the Nginx portion which consisted of created a configuration file that the Dockerfile can reference and then replace the default one with ours.

Ours looks like this:

~~~
server {
    listen       80;
    server_name  staging.zephyr90.com www.staging.zephyr90.com blog.staging.zephyr90.com;

    location / {
        root   /var/www/site;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
~~~

I've taken out all the commented sections but it points Nginx to our public folder that Hugo creates. These configuration file and the directory is all the Dockerfile needs in order to properly run the image. We have uploaded this Dockerfile with it's necessary files into our GitHub blog repository. This is then used in the CircleCI to build and then tag the container.

Next we have been trying to work on the AWS lambda. This task has been rather difficult for us, we don't necessarily know exactly how to use lambda and every attempt to try to get the scripts to work have failed. We first tried to look through the public repository that Lambda has itself and found a script that sounded like what we needed.
~~~
import os
import boto3
from botocore.exceptions import ClientError

def lambda_handler(event, context):
    if event['detail']['responseElements'] == None: return

    cluster = os.environ['CLUSTER']
    tagLatest = os.environ['TAG_LATEST'] == 'true'
    deployLatest = os.environ['DEPLOY_LATEST'] == 'true'
    if tagLatest and deployLatest: deployLatest = False

    region = event['region']
    repositoryName = event['detail']['responseElements']['image']['repositoryName']
    registryId = event['detail']['responseElements']['image']['registryId']
    image = f'{registryId}.dkr.ecr.{region}.amazonaws.com/{repositoryName}'
    imageTag = event['detail']['responseElements']['image']['imageId']['imageTag']
    imageManifest = event['detail']['responseElements']['image']['imageManifest']

    if not deployLatest and imageTag == 'latest':
        print(f'Skipped {image}:{imageTag}')
        return

    ecs = boto3.client('ecs', region_name=region)
    ecr = boto3.client('ecr', region_name=region)

    def get_task_definitions():
        response = ecs.list_task_definition_families(status='ACTIVE')
        families = response['families']
        while ('nextToken' in response):
            response = ecs.list_task_definition_families(nextToken=response['nextToken'])
            families.append(response['families'])

        taskDefinitions = [ecs.describe_task_definition(taskDefinition=family)['taskDefinition'] for family in families]
        return [
            taskDefinition for taskDefinition in taskDefinitions
            if any([
                c for c in taskDefinition['containerDefinitions']
                if c['image'].startswith(image) and not c['image'].endswith(f':{imageTag}')
            ])
        ]

    def update_task_definition(taskDefinition, newTaskDefinitions, image, imageTag):
        family = taskDefinition['family']
        containerDefinitions = taskDefinition['containerDefinitions']
        print(f'Update task definition: {family}')
        [
            update_container_definition(containerDefinition, image, imageTag)
            for containerDefinition in containerDefinitions
            if containerDefinition['image'].startswith(image)
        ]

        response = ecs.register_task_definition(
            family=family,
            taskRoleArn=taskDefinition['taskRoleArn'],
            containerDefinitions=containerDefinitions,
            volumes=taskDefinition['volumes'],
            placementConstraints=taskDefinition['placementConstraints'],
            requiresCompatibilities=taskDefinition['compatibilities'])
        oldTaskDefinitionArn = taskDefinition['taskDefinitionArn']
        newTaskDefinitionArn = response['taskDefinition']['taskDefinitionArn']
        newTaskDefinitions[strip_arn(oldTaskDefinitionArn)] = newTaskDefinitionArn
        return newTaskDefinitionArn

    def update_container_definition(containerDefinition, image, imageTag):
        print(f'Update container definition to {image}:{imageTag}')
        containerDefinition['image'] = f'{image}:{imageTag}'

    def get_services(cluster):
        response = ecs.list_services(cluster=cluster)
        service_arns = response['serviceArns']
        while ('nextToken' in response):
            response = ecs.list_services(cluster=cluster, nextToken=response['nextToken'])
            service_arns.append(response['serviceArns'])
        return [
            service for service in ecs.describe_services(cluster=cluster, services=service_arns)['services']
            if service['status'] == 'ACTIVE'
        ]

    def update_service(service, newTaskDefinitionArn):
        serviceArn = service['serviceArn']
        print(f'Update service {serviceArn} with {newTaskDefinitionArn}')
        ecs.update_service(cluster=cluster, service=serviceArn, taskDefinition=newTaskDefinitionArn)
    
    def tag_latest():
         print(f'Tag {image}:{imageTag} with latest')
         try:
             ecr.put_image(registryId=registryId, repositoryName=repositoryName, imageManifest=imageManifest, imageTag='latest')
         except ClientError as e:
            if e.response['Error']['Code'] != 'ImageAlreadyExistsException':
                raise
            print(f'Image {image}:{imageTag} is already tagged with latest')
         

    def strip_arn(arn):
        return arn[:arn.rindex(":")]

    # Update Task Definitions
    taskDefinitions = get_task_definitions()

    newTaskDefinitions = {}
    [update_task_definition(taskDefinition, newTaskDefinitions, image, imageTag) for taskDefinition in taskDefinitions]

    if newTaskDefinitions:
        services = get_services(cluster)
        # Update Services
        [
            update_service(service, newTaskDefinitions[strip_arn(service['taskDefinition'])])
            for service in services
            if strip_arn(service['taskDefinition']) in newTaskDefinitions.keys()
        ]

~~~
 Trying to implement the script was a failure for unknown reasons. We were trying to setup up the Global variables but we kept running into errors referring to events. Which seems like the common issue with most of our errors are the event triggers. I don't have a proper understanding of the error.

Unfortunately talking to other groups for suggestions on how to do the Lambda portion they suggested not using that public repository and try to do it yourself. This is because this script doesn't necessarily do what we were tasked to do. The public script looks for a image being pushed into the ECR then pushes it to the cluster. To me it sounds like it matches the task but people are suggesting that it needed to use s3 to look for a trigger as well as it needed to look for tags and not just a trigger of a push image.

We are currently still trying to get the Lambda working, we have 3 people working on it and hopefully we can get it done before tonight.
