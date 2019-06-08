# Wirecutter-devops-assigment
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

# Choosing a provisioning tool and some Prerequisites - 

This was easy, as I've never used any of the suggested tools in production before, only as a lab excercise while learning the basics of AWS.  For this reason I chose to use the most familiar to me - CloudFormation.  Additionally, since its the built in service for automated provisioning on AWS, it is provived free of charge for use (you only get charged for resources you deploy/use, etc.).

Having made this decision, it was a matter of just diving in a breaking down the tasks, and making sure they'd be covered in the template. I'm learning all this on the fly, so I chose to use a template to work from that was already designed to deploy an EC2 instance based on an AMI of Amazon Linux, a distro also easiest to work with based on its support of the tools needed throughout the rest of this exercise. This template is based on the ones in the lessons found here:
https://github.com/ACloudGuru/intro-to-CloudFormation_AC

Since I was given the private/public keypair (ssh key) for the user in our example, the prequisite to use it was to upload the public key into my AWS account (account from which one would launch cloudformation template), and have it ready. 
Adding external key pairs is covered in the AWS Documention here:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws


#Meeting the challenge
Let's take a look at how this template tries to acheive meeting the various requirements, and also where it falls short.
First, how do we "Add a group called wirecutter
Add a user dave to the wirecutter group" ?
This is addressed in the template here in lines 66-69:

 /# add user dave, create group wirecutter, and add dave to wirecutter group:
  
            groupadd wirecutter
            useradd -d /home/dave -g wirecutter -m -s /bin/bash dave
            
By putting these commands inside the UserData section of the Properties section, called by the intrinsic Function('Fn::Base64'), CF will pass these and run them as root during launch of our EC2 instance.

The need to "Install the private SSH key below for the user dave" is done by uploading as mentioned above and then by specifying the default keyname in our templates Parameters section here (lines 5-8):

   KeyName:
      Description: Name of an existing EC2 KeyPair to enable SSH access into the server
      Type: AWS::EC2::KeyPair::KeyName
      Default: "dave-key-pair"



Next we have the objective to "Only allow dave to have SSH access from the following IPV4 addresses
192.0.2.0
192.0.2.128
198.51.100.192
"

This is done by creating secutity group ("MySecurityGroup") in the Resources, which we can assign a ingress policy via the SecurityGroupIngress spec (line 76).  The creation of the group, as seen in our inspection of the EC2 instance > Network Security Groups shows, meets our requirements - 
[[https://github.com/jeckertchu/wirecutter-devops-assignment/blob/master/mysecuritygroup.jpg]]


 


