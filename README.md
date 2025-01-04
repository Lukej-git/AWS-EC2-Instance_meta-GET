# AWS-EC2-Instance_meta-GET
# bash script that reads and store instance meta to use in print when visiting via public IP
# Refer to https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html for more metadata
# Copy below and paste into Advanced Detail > User Data

#!/bin/bash
yum update -y
yum install httpd -y

systemctl start httpd
systemctl enable httpd

TOKEN=`curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
INSTANCE_AZ=`curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/placement/availability-zone`
INSTANCE_ID=`curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id`
AMI_LAUNCH_INDEX=`curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/ami-launch-index`

echo "<h1>Hello from $INSTANCE_ID</h1>" | sudo tee /var/www/html/index.html
