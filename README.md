# aws-cli

### ec2_create_security_group :: without --vpc-id then default vpc

```
 aws ec2 create-security-group --group-name {{groupName}} --vpc-id {{vpcId}} --description "{{description :: required}}"
```

### ec2_set_security_group_inbound_rule :: set cidr for security && open 22 port for ssh
```
aws ec2 authorize-security-group-ingress --group-name {{groupName}} --protocol tcp --port 22 --cidr {{cidr ex:0.0.0.0/0}}
```

### ec2_create_key_pair
```
aws ec2 create-key-pair --key-name {{keyName}} --query 'KeyMaterial' --output text > {{keyName.pem}}
```

### ec2_set_mode_to_key_pair
```
chmod 400 {{keyName.pem}}
```




### ec2_launch_instance :: ubuntu 18.04 AMI - ami-06e7b9c5e0c4dd014
```
# get security group id 
aws ec2 describe-security-groups --group-name={{groupName}}

# launch instance
aws ec2 run-instances --image-id {{AMI}} \
--subnet-id {{subnetId}} \
--security-group-ids {{securityGroupId}} \
--count {{instanceCount}} \
--instance-type {{instanceType}} \
--key-name {{keyName}} \
--query 'Instances[0].InstanceId'

# get instance public IP
aws ec2 describe-instances \
--instance-ids {{instanceId}} \
--query 'Reservations[0].Instances[0].PublicIpAddress'

# set local DNS in mac - ex:: {{public ip}} {{alias}}
sudo vi /etc/hosts
```

### connect to instance
```
ssh -i {{keyName.pem}} {{user}}@{{publicIp}}
```


