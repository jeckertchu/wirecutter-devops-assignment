# Wirecutter-devops-test
This is assignment for which this project is based.
I was tasked with the following assigment objectives (below text is directly excertped from original email):

  Use a provisioning/configuration management tool of your choice – such as Ansible, Terraform, or AWS CloudFormation – to set up a new Linux server meeting the requirements below. If this were run in our production environment, it would be a new AWS EC2 instance.

Please create a GitHub repository containing your script(s) and a README [ed note: this file]. 
The README file should describe your solution along with any interesting details you’d love to highlight for us.

Use your favorite server distro and explain your choice in the README
Add a group called wirecutter
Add a user dave to the wirecutter group
Install the private SSH key below for the user dave
Only allow dave to have SSH access from the following IPV4 addresses
192.0.2.0
192.0.2.128
198.51.100.192
Install nginx and stand up a “Hello World” static HTML page at /hello
Configure nginx to display content from http://example.com/ at /example
Configure nginx to protect the content at /example with basic auth
Username: admin
Password: password
Configure logrotate to rotate nginx access logs hourly (using cron)

-------------- end excerpt --------------

I was also given a keypair (private and public) for user dave as referenced above

# Choosing a provisioning tool

This was easy, as I've never used any of the suggested tools in production before, only as a lab excercise while learning the basics of AWS.  For this reason I chose to use the most familiar to me - CloudFormation.  Additionally, since its the built in service for automated provisioning on AWS, it is provived free of charge for use (you only get charged for resources you deploy/use, etc.).

Having made this decision, it was a matter of just diving in a breaking down the tasks, and making sure they'd be covered in the template. Since I'm learning on the go, I chose to use a template to work from that was designed to deploy an EC2 instance based on an AMI of Amazon Linux, a distro also easiest to work with based on its support of the tools needed throughout the rest of this exercise.
