---
title: "Brian's Blog. Week 9 Spring"
date: 2019-29-1T15:53:17-07:00
draft: false
layout: 'posts'
tags: ["Brian Sandoval", "saps", "week9"]
---
# Brian's Blog Week9 Spring
## This week's overview
### Promehteus setup with Node Exporter
The assignment this week showed me something new when it came to setting up a prometheus metrics graph. I had already experience setting up prometheus but was able to learn about how node exporter provides other metrics for itself. I was able to setup the graph but didnt realize that you had to have both files running at the same time to see the options displayed on the browser the fix was simple jsut had to use docker exec -it container_name bash to have another terminal within the container.
### Aurora RDS Cluster
This week I ended up setting up grafana just showing an initial graph metrics that is keeping count of the cpu per seconds. Right now I am just helping Erik out with setting up metrics for the rest of our other instances. On top of this I have been wanting to create an rds cluster which I have started looking into. So far what I have gotten from the documentation in terraform we can spin up an aurora sql cluster as such:
```
resource "aws_rds_cluster" "default" {
  cluster_identifier      = "aurora-cluster-demo"
  engine                  = "aurora-mysql"
  availability_zones      = ["us-west-2a", "us-west-2b", "us-west-2c"]
  database_name           = "mydb"
  master_username         = "user"
  master_password         = "passwd"
  backup_retention_period = 5
  preferred_backup_window = "07:00-09:00"
}
```
An instance startup below:
```
resource "aws_rds_cluster_instance" "cluster_instances" {
  count              = 2
  identifier         = "aurora-cluster-demo-${count.index}"
  cluster_identifier = "${aws_rds_cluster.default.id}"
  instance_class     = "db.r4.large"
}
```
After I finish setting this up I want to bconsult with my team to see if there is a lambda funtion that they perhaps want to test or create so we can configure things more.