---
title: "Anthony Week 16"
date: 2019-01-25T17:18:04-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 16"]
layout: 'posts'
---
# Winter review
Just because it's winter break doesn't mean I won't touch the senior design project. The break gave me valuable time to reflect on the project without the stress of other classes and responsibilities. Our team was still executing tasks over break so we can prepare for the semester. During the winter I accomplished a few tasks/improvements

* Migrated from a Lightsail instance to EC2 spot instance
* Swapped Apache for Nginx
* Deployed an ELK stack and began monitoring the servers
* Ansible and Terraform for all changes
* CI/CD pipeline to deploy into an ECR
* Some backend programming

## EC2 Spot instance
I've heard of spot instances and how it's far less expensive compared to an on-demand spot instance. There's not much documentation on the entire process of creating a spot request on Terraform. Also, there are no official modules for creating one so I made one a resource block from scratch. The provisioner `local-exec` is used because spot instances with Terraform don't tag the name properly. The prices for a t.2 spot instance have been stable for the past 3 months so I'm not too worried about them going down.

```json
resource "aws_spot_instance_request" "cp_dev_server" {
  ami                         = "${var.ami_id}"
  spot_price                  = "0.0045"
  instance_type               = "t2.micro"
  key_name                    = "${var.key_name}"
  monitoring                  = true
  associate_public_ip_address = true
  count                       = 1
  wait_for_fulfillment        = true
  user_data                   = "${file("../cloud-init.conf")}"
  security_groups             = ["${module.web_server_sg.this_security_group_id}"]
  subnet_id                   = "${data.terraform_remote_state.vpc.public_subnet_a}"

  ebs_block_device = [{
    device_name = "/dev/sdh"
    volume_size = "20"
    volume_type = "gp2"
  }]

  provisioner "local-exec" {
    command = "aws ec2 create-tags --resources ${aws_spot_instance_request.cp_dev_server.spot_instance_id} --tags Key=Name,Value=cp-dev-server-${count.index}"
  }

  tags {
    Name = "cp_dev_server"
  }
}
```

## ELK Stack
It took me months to finally implement the ELK stack. I wrote documentation [here](https://digitalsoba.github.io/classroom-profiles-ops/ops/elastic-stack/) which further explains the installation process with Ansible. The ELK stacks allow me to gain an overview of my infrastructure.

## Backend task
I've created a quick database migration for a users table on the database. After the schema has been created, I can run `php artisan migrate` to create the table 

```php
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('user_id');
            $table->string('display_name');
            $table->string('first_name');
            $table->string('last_name');
            $table->string('email')->unique();
            $table->string('affiliation');
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }
```

I've also created a seeder to create a test user 
```php
<?php
 use Illuminate\Database\Seeder;
 class UsersTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        DB::table('users')->insert([
            'display_name' => 'steve_fitzgerald',
            'first_name' => 'Steve',
            'last_name' => 'Fitzgerald',
            'affiliation' => 'Student',
            'email' => str_random(10).'@gmail.com',
            'password' => bcrypt('secret'),
        ]);
    }
}
```

## What's next?
My next blog post will cover my CI / CD pipeline and how I was able to pull the secret.env file for the build before it was pushed to an ECR. I'm also going to work on a method for gathering metrics. Prometheus and Grafana are quite popular, but I know that ELK can also collect metrics, but at a price.