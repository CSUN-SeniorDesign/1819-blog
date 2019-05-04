---
title: "Matabit Mario Week 29"
date: 2019-05-03T19:39:59-07:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 29"]
layout: 'posts'
---

# Week 29

This week I mainly focused on installing and configuring Corosync and Pacemaker. Me and john were tasked with installing and configuring them, Mark was tasked with working on DHCP and DNS, while Thomas was tasked with testing and documenting the firewall as well as firewall builder. 

### Corosync and Pacemaker

Corosync is an open source program that provides cluster membership and messaging capabilities, often referred to as the messaging layer to client servers. 

Pacemaker is an open source cluster resource manager (CRM), it is a system that coordinates resources and services that are managed and made highly available by a cluster. Corosync enables servers to communicate as a cluster, while Pacemaker provides that ability to control how the cluster behave. 

We decided to go with Corosync and Pacemaker to use cluster management to manage our floating IP instead of the fail over service we previously had which was keepalived. 

First task was to install Corosync and Pacemaker by following simple apt-get install commands. The next step after that was to configure the corosync.conf file. 
The corosync.conf file looks as so: 
```
# Please read the corosync.conf.5 manual page
totem {
	version: 2

	# Corosync itself works without a cluster name, but DLM needs one.
	# The cluster name is also written into the VG metadata of newly
	# created shared LVM volume groups, if lvmlockd uses DLM locking.
	# It is also used for computing mcastaddr, unless overridden below.
	cluster_name: techlabfw
	transport: udpu

	# How long before declaring a token lost (ms)
	token: 3000

	# How many token retransmits before forming a new configuration
	token_retransmits_before_loss_const: 10

	# Limit generated nodeids to 31-bits (positive signed integers)
	clear_node_high_bit: yes

	# crypto_cipher and crypto_hash: Used for mutual node authentication.
	# If you choose to enable this, then do remember to create a shared
	# secret with "corosync-keygen".
	# enabling crypto_cipher, requires also enabling of crypto_hash.
	# crypto_cipher and crypto_hash should be used instead of deprecated
	# secauth parameter.

	# Valid values for crypto_cipher are none (no encryption), aes256, aes192,
	# aes128 and  3des. Enabling crypto_cipher, requires also enabling of
	# crypto_hash.
	crypto_cipher: none

	# Valid values for crypto_hash are  none  (no  authentication),  md5,  sha1,
	# sha256, sha384 and sha512.
	crypto_hash: none

	# Optionally assign a fixed node id (integer)
	# nodeid: 1234

	# interface: define at least one interface to communicate
	# over. If you define more than one interface stanza, you must
	# also set rrp_mode.
	interface {
		# Rings must be consecutively numbered, starting at 0.
		ringnumber: 0
		# This is normally the *network* address of the
		# interface to bind to. This ensures that you can use
		# identical instances of this configuration file
		# across all your cluster nodes, without having to
		# modify this option.
		bindnetaddr: 130.166.240.18
		# However, if you have multiple physical network
		# interfaces configured for the same subnet, then the
		# network address alone is not sufficient to identify
		# the interface Corosync should bind to. In that case,
		# configure the *host* address of the interface
		# instead:
		# bindnetaddr: 192.168.1.1
		# When selecting a multicast address, consider RFC
		# 2365 (which, among other things, specifies that
		# 239.255.x.x addresses are left to the discretion of
		# the network administrator). Do not reuse multicast
		# addresses across multiple Corosync clusters sharing
		# the same network.
		# mcastaddr: 239.255.1.1
		# Corosync uses the port you specify here for UDP
		# messaging, and also the immediately preceding
		# port. Thus if you set this to 5405, Corosync sends
		# messages over UDP ports 5405 and 5404.
		broadcast: yes
		mcastport: 5405
		# Time-to-live for cluster communication packets. The
		# number of hops (routers) that this ring will allow
		# itself to pass. Note that multicast routing must be
		# specifically enabled on most network routers.
		#ttl: 1
	}
}

logging {
	# Log the source file and line where messages are being
	# generated. When in doubt, leave off. Potentially useful for
	# debugging.
	fileline: off
	# Log to standard error. When in doubt, set to no. Useful when
	# running in the foreground (when invoking "corosync -f")
	to_stderr: no
	# Log to a log file. When set to "no", the "logfile" option
	# must not be set.
	to_logfile: yes
	logfile: /var/log/corosync/corosync.log
	# Log to the system log daemon. When in doubt, set to yes.
	to_syslog: yes
	# Log with syslog facility daemon.
	syslog_facility: daemon
	# Log debug messages (very verbose). When in doubt, leave off.
	debug: off
	# Log messages with time stamps. When in doubt, set to on
	# (unless you are only logging to syslog, where double
	# timestamps can be annoying).
	timestamp: on
	logger_subsys {
		subsys: QUORUM
		debug: off
	}
}

quorum {
	# Enable and configure quorum subsystem (default: off)
	# see also corosync.conf.5 and votequorum.5
	provider: corosync_votequorum
	#expected_votes: 2
	#two_node: 1
}

nodelist {
  node {
    ring0_addr: 130.166.240.19
    name: primary
    nodeid: 1
  }
  node {
    ring0_addr: 130.166.240.20
    name: secondary
    nodeid: 2
  }	
}
```
The first change that was made was adding the cluster nodes in a nodelist that points to firewall A and Firewall b. Next we un-commneted several lines from the logging section in order to log the actions happening. After that I added the floating IP address to the bindnetaddr to specify the fail-over. 
In addition to the installation and configuration I took care of the keygen for the authkey on both of the firewalls as well as included these changes to the conf file. One thing we also had to do was open ports 5404-5406. The reason for this is because corosync needs to use ntp to synchronize both firewalls so that the changes made would be reflected on the other server. This means we had to go into firewall builder to open these ports which Thomas took care of, instead of using the iptables command. 

Once both services were installed there are several services we came across that we would need for this to work. These services are resource stickiness, crmsh, pcs, and resource agents.

#### Resource Stickiness
Resource Stickiness is highly desirable to prevent healthy resources from being moved around the cluster. Moving resources almost always requires a period of downtime. For complex services such as databases, this period can be quite long.

To address this, Pacemaker has the concept of resource stickiness, which controls how strongly a service prefers to stay running where it is. You may like to think of it as the "cost" of any downtime. By default, Pacemaker assumes there is zero cost associated with moving resources and will do so to achieve resource placement. We can specify a different stickiness for every resource, but it is often sufficient to change the default.

#### CRMSH
Crmsh which john took care of is a cluster management shell that is used for the majority of pacemakers operations. 

#### PCS

Pcs stands for pacemaker/corosync configuration system, it simply controls and configures corosync and pacemaker. 

#### Resource Agent
Resource agent is a standardized interface for a cluster resource. In translates a standard set of operations into steps specific to the resource or application, and interprets their results as success or failure.