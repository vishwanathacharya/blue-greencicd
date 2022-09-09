#!/bin/bash -xe

apt update -y
apt install apache2 -y
cd /var/www/html/
rm -rf index.html
echo 'ROBOT' >> /var/www/html/index.html 
systemctl restart apache2

apt-get update
apt-get install -y ruby
wget https://aws-codedeploy-ap-south-1.s3.amazonaws.com/releases/codedeploy-agent_1.0-1.1597_all.deb
dpkg-deb -R codedeploy-agent_1.0-1.1597_all.deb codedeploy-agent_1.0-1.1597_ubuntu20
sed 's/2.0/2.7/' -i ./codedeploy-agent_1.0-1.1597_ubuntu20/DEBIAN/control
dpkg-deb -b codedeploy-agent_1.0-1.1597_ubuntu20
dpkg -i codedeploy-agent_1.0-1.1597_ubuntu20.deb
systemctl start codedeploy-agent
