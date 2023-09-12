# Deployment Guide
Resource Deployment using AWS Management Console (GUI)

1. Login into Management Console
2. Go to EC2 page 
3. Click **Launch Instance**  
    <img src="Pictures/1. Launch Instance.png" width="350" height="350" alt="Launch Instance">

4. Write down EC2 name
5. Application & OS Image (AMI)  
   i. Amazon Linux & Amazon Linux 2023 AMI (**Free Tier Eligible**)  
   ii. Architecture : 64-bit (x86)  
   <img src="Pictures/2. AMI.png" width="350" height="350" alt="AMI">

6. Instance type
   <br>i. t2.micro (**Free Tier Eligible**)  
7. Create key pair for login (if neccesary) <br>
    ![Key Pair](<Pictures/3. Key Pair.png>)
    > EC2 instance login via *ssh*, etc, or select **proceed without a key pair**

8. Network settings <br>
   ![Network settings](<Pictures/4. Network settings.png>) <br>
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
    export AWS_DEFAULT_REGION=<INSERT REGION HERE>    
    export DYNAMO_MODE=on    
    FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80 
    ```
    ![User Data Script](<Pictures/5. User Data.png>)

11. Wait for instance to complete deployment <br>
    **Instance Status** & **Status Check**
    ![Instance (Pending)](<Pictures/6. Instance Status.png>)
    ![Instance (Complete)](<Pictures/7. Instance Status (Complete).png>)


