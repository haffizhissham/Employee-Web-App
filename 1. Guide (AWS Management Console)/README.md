# Deployment Guide
Resource Deployment using AWS Management Console (GUI)

_________________

### 1) EC2
1. Login into Management Console
2. Go to EC2 page 
3. Click **Launch Instance**  
    <img src="Pictures/EC2/1. Launch Instance.png" width="350" height="350" alt="Launch Instance">

4. Write down EC2 name
5. Application & OS Image (AMI)  
   i. Amazon Linux & Amazon Linux 2023 AMI (**Free Tier Eligible**)  
   ii. Architecture : 64-bit (x86)  
   <img src="Pictures/EC2/2. AMI.png" width="350" height="350" alt="AMI">

6. Instance type
   <br>i. t2.micro (**Free Tier Eligible**)  
7. Create key pair for login (if neccesary) <br>
    ![Key Pair](<Pictures/EC2/3. Key Pair.png>)
    > EC2 instance login via *ssh*, etc, or select **proceed without a key pair**

8. Network settings <br>
   ![Network settings](<Pictures/EC2/4. Network settings.png>) <br>
   i. Default **VPC** & **Subnet** <br>
   ii. Auto-assign public IP <br>
   iii. Firewall/Security Groups, create if no existing group <br>
   iv. Uncheck/check **SSH** as applicable (if check, set IP to access instance via SSH) <br>
   v. Allow **HTTPS** and **HTTP** from the internet <br>
9. Storage <br>
   i. Default 8GiB, gp3
10. Advance details <br>
    i. select role on IAM Instance Profile <br>
    ii. Paste script on **user data** section <br>
    ```
    #!/bin/bash -ex    
    wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip    
    unzip FlaskApp.zip    
    cd FlaskApp/    
    yum -y install python3-pip    
    pip install -r requirements.txt    
    yum -y install stress    
    export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}    
    export AWS_DEFAULT_REGION=us-west-2    
    export DYNAMO_MODE=on    
    FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80 
    ```
    ![User Data Script](<Pictures/EC2/5. User Data.png>)

11. Click launch instance
    <br>![Launch](<Pictures/EC2/8. Launch Instance.png>)

12. Wait for instance to complete deployment, **Instance Status** & **Status Check**
    ![Instance (Pending)](<Pictures/EC2/6. Instance Status.png>)
    ![Instance (Complete)](<Pictures/EC2/7. Instance Status (Complete).png>)


_________________

### 2) S3 Bucket
1. Logi

_________________

### 3) Dynamo DB
1. Logi


_________________

#### Misc.

##### SSH with EC2 Instance
```
ssh -i /path/key-pair-name.pem ec2-user@Public IP
```
![SSH](<Pictures/Misc./1. SSH.png>)
Reference : [Connect to your Linux instance using an SSH client](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-ssh.html)


 