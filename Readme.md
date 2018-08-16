### Re-create jenkins infrustructure

1. Referesh aws credential

> [Refer this instruction:](https://github.com/CMSgov/qpp-cost-scoring#aws-mfa)

```
  ## make sure you are running this command from the root path of qpp-cost-scoring repository
  docker-compose run mvnw aws-mfa --profile aws-hhs-cms-amg-qpp-costscoring
```

2. Run docker compose

> [Refer this instruction:](https://github.com/CMSgov/qpp-cost-scoring/tree/master/terraform#run-a-terraform-module)

```
  ## change to 'terraform' directory
  cd terraform

  ## run dockercompose command
  docker-compose run tg
```

This will put you inside the container.

```
   [root@9a6793d3e670 terraform]# pwd
   /terraform

```

3. Terraform plan

```
  ## change to jenkins terraform directory
  cd tools/jenkins/

  ## change to jenkins directory
  
  terragrunt plan
```

4. Terraform apply

One you verify the terraform plan, you can execute terraform apply

```
  ## make sure you are in jenkins terraform directory '/terraform/tools/jenkins'
  terragrunt apply
```

5. Cloud-int log

You can look at the realtime logs of terraform's provisioning by SSH'ing to the instance and executing this command

```
  ## ssh to the jenkins instance
  ssh -i ~/.ssh/id_rsa ec2-user@<JENKINS_INSTANCE_IP_ADDRESS>
   
  ## tail the cloud-init log file
  tail -f /var/log/cloud-init-output.log 
```

### Update Jenkins

1. Right click on the download link and “copy link address”

<kbd>
  <img src="copy-link.png">
</kbd>

2. Log in into jenkins container

```
  ## ssh to jenkins ec2 instance
  ssh -i ~/.ssh/id_rsa ec2-user@<JENKINS_INSTANCE_IP_ADDRESS>

  ## ssh to jenkins container
  docker exec -it <JENKINS_CONTAINER_ID> /bin/bash
```

3. Download the update with the address you copied in step 1

```
  ## inside the container, using 2.89.2 as example
  wget http://updates.jenkins-ci.org/download/war/2.89.2/jenkins.war
```

4. Move it to the correct place

```
  mv ./jenkins.war /usr/share/jenkins
```

5. Change permission

```
  chmod 777 /usr/share/jenkins/jenkins.war
```

6. Eixt container and restart the container

```
  ## exit contaienr (inside container)
  exit

  ## restart container (from your server)
  docker container restart jenkins
```
