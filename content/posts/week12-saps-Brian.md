# Brian's Blog Week 12 Spring
## Overview this week
This week was a bit hectic in the beginning of it since our infrastructure went down at the beginning of the week. For some reason our production site was broken and we had to spend around 2 days trying to figure out how we could bring it back up. The issue seemed to be that there was not enough memory for our website so we had to increase the size. It was a bit odd to see this error becaus we had allocated enough space prior to this plus the default size that had been put in placed seemed to increase. Shahed was able to get past the problem he got the website up and running. Currently I have just finished the documentation for RDS I still want to be able to see if there is some way that I can build upon the current project since things seemed to have gotten a bit cold.
## Future Plans/ Interests
I have interest in learning how a redshift cluster is setup just becausw I am looking to see what things we can implment plus I know that it is heavily used due to the fact that you are able to analyze data much more efficiently.
Initialize a redshift cluster:
```
resource "aws_redshift_cluster" "default" {
  cluster_identifier = "tf-redshift-cluster"
  database_name      = "mydb"
  master_username    = "foo"
  master_password    = "Mypass
  node_type          = "dc1.large"
  cluster_type       = "single-node"
}
```
Use of a security group which still need to be configured with the right ingress and egress for redshift security.
```
resource "aws_redshift_security_group" "default" {
  name = "redshift-sg"

  ingress {
    cidr = "10.0.0.0/24"
  }
}
```
I am looking to use amazon SNS which is the simple notification service that allows for us to get notified of changes that are going on.I as well want to configure this on our in current infrastructure so that we are not blindsided as we were by the issue that arised earlier this week.

'''
resource "aws_sns_topic" "default" {
  name = "redshift-events"
}

resource "aws_redshift_event_subscription" "default" {
  name          = "redshift-event-sub"
  sns_topic_arn = "${aws_sns_topic.default.arn}"

  source_type = "cluster"
  source_ids  = ["${aws_redshift_cluster.default.id}"]

  severity = "INFO"

  event_categories = [
    "configuration",
    "management",
    "monitoring",
    "security",
  ]

  tags = {
    Name = "default"
  }
}
```
The following event categories that have been listed will send notifications about an alert that end sup affecting any of the categories listed and just for test purposes everything has been set to as an information on the severity part.