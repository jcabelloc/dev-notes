# AWS Notes


### Set up a key pair
* Saved on: C:\jcabelloc\credentials\aws

## Amazon ECS Command Line Interface
* https://github.com/aws/amazon-ecs-cli
* https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI.html

### Instalar ECS-cli
* https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html

* Step 1: Download the Amazon ECS CLI
```powershell
PS C:\> New-Item ‘C:\jcabelloc\programs\Amazon\ECSCLI’ -type directory
PS C:\> Invoke-WebRequest -OutFile ‘C:\jcabelloc\programs\Amazon\ECSCLI\ecs-cli.exe’ https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-windows-amd64-latest.exe
```

* Step 2: Edit the environment variables and add C:\jcabelloc\programs\Amazon\ECSCLI\ to the PATH variable field,
* Step 3: Verify installation
```cmd
ecs-cli --version
```

### Configuring the Amazon ECS CLI
* https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_Configuration.html

* To configure the Amazon ECS CLI
```cmd
ecs-cli configure profile --profile-name jcabelloc2312 --access-key %AWSAccessKeyId% --secret-key %AWSSecretKey%

ecs-cli configure --cluster sms-fintech-ecs --default-launch-type EC2 --region 	us-west-2 --config-name config-sms-fintech-ecs
```

### 
