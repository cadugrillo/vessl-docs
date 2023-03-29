+++
title = "Installation"
description = ""
weight = 1
+++

{{< lead >}}
Installing Vessl is easy. Since it is also a container you just need to run it.
{{< /lead >}}

## Prerequisites
In order to run Vessl you need to have Docker Engine installed in your system.  
For a complete guide on how to install Docker in your target system visit <a href="https://docs.docker.com/engine/install/" target="_blank">Install Docker Engine</a>.

## How to run
Create a dedicated network for Vessl container.
{{< code lang="bash" >}}
docker network create vessl-default
{{< /code >}}
Now pull the latest image and run it with the following arguments.
{{< code lang="bash" >}}
docker run -dp 443:443 --name=vessl --restart=always \
  -v vessl-database:/database \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --network=vessl-default vessl/vessl:latest
{{< /code >}}

### Check if it is running
On your web browser navigate to <a href="https://localhost" target="_blank">localhost</a> or https://"your-ip-address".  
You should see the login page. Use the default username and password displayed below.  
default Username: master  
default Password: cgMaster@3306

## (Optional) Manage Network Configuration
If you are running Vessl on a Linux target system that uses /etc/network/interfaces for network configuration, you can manage interfaces from Vessl by adding the following line to the run command.
{{< code lang="bash" >}}
-v /etc/network:/etc/network
{{< /code >}}
So the complete run command with all arguments would look like the below example.
{{< code lang="bash" >}}
docker run -dp 443:443 --name=vessl --restart=always \
  -v vessl-database:/database \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /etc/network:/etc/network \
  --network=vessl-default vessl/vessl:latest
{{< /code >}}

## (Optional) Manage Reboot and Shutdown
If you are running Vessl on a Linux target system you can control reboot and restart of the target system using this auxiliary container using the following run command.
{{< code lang="bash" >}}
docker run -d --name=vessl-host-control \
  --restart=always --privileged --pid=host \
  --network=vessl-default vessl/host-control:latest
{{< /code >}}