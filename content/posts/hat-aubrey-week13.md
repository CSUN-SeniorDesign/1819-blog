---
title: "Hat Aubrey Week13"
date: 2018-11-23T17:46:42-08:00
draft: false
---

# Applying ALB and ACM #

This week I have implemented the ALB while utilizing certificate thru Amazon Certificate Manager. The ACM allows us to automate request and generation of certificate. We utilize Route 53 to prove the ownership of the domain. The setup that we did is a Proof of Concept as the final product will have a different domain name. 
 

#Request certificate to be used by loadbalancer
resource "aws_acm_certificate" "cert" {
  domain_name = "legalforms.info"
  subject_alternative_names = ["*.legalforms.info"]
  validation_method = "DNS"
}
#Create validation record for the certificate
resource "aws_route53_record" "cert_validation" {
  name    = "${aws_acm_certificate.cert.domain_validation_options.0.resource_record_name}"
  type    = "${aws_acm_certificate.cert.domain_validation_options.0.resource_record_type}"
  zone_id = "${aws_route53_zone.main.zone_id}"
  records = ["${aws_acm_certificate.cert.domain_validation_options.0.resource_record_value}"]
  ttl     = 60
}
#validate the certificate using the validation records created above
resource "aws_acm_certificate_validation" "cert" {
  certificate_arn         = "${aws_acm_certificate.cert.arn}"
  validation_record_fqdns = ["${aws_route53_record.cert_validation.fqdn}"]
}