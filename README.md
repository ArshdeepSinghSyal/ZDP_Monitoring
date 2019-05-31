# Monitoring & Analysis

![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/zaloni.jpg)

## Project Objective

Our goal was to monitor apache server (httpd) using metricbeat and use elasticsearch to store and kibana to visualize the collected metric on a distributed environment over the aws cloud.

The following diagram shows the architecture of how this has been achieved:

![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/Architecture.png)

- Metricbeat and apache httpd are installed and configured on the web server.

- Metricbeat is configured to monitor httpd.

- Elasticsearch and Kibana are installed and configured on the visual server.

- Metricbeat collects metrics by monitoring httpd and continuously sends them to elasticsearch in real-time.

- Elasticsearch stores metrics sent to it by metricbeat.

- Kibana fetches metrics from elasticseach and creates visualizations.

## Applications of Project

- Kibana is used to create visualizations which can be imported into the client's kibana dashboard. This would allow the client to view informative visuals about the services being monitored.

- Client can also create new visualizations and add them to his kibana dashboard.

- The following visualizations were created using kibana:

![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/v1.png)
<br>
![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/v2.png)
<br>
![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/v3.png)
<br>
![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/v4.png)
<br>
![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/v5.png)

The link to the [presentation](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/PPT.key)

## Running the Project

### Pre-requisites

- Python = 2.7
- Ansible >= 2.6.5
- boto
- boto3
- nose
- tornado
- AWS CLI

### Steps to instals pre-requisites:

```shell
easy_install pip
pip install ansible==2.6.5
pip install nose
pip install tornado
pip install boto
pip install boto3
```

### Steps to install and configure AWS CLI:
```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

**Make changes to the following files:**
<br>a) ~/.aws/config
<br>b) ~/.aws/credentials

**Changes to ~/.aws/config**
```
[default]
role_arn = arn:aws:iam::<account_id_number>:role/<role_name>
output = json
region = <region name> ex. us-east-1
source_profile = <aws username>

[profile <aws username>]
output = json
region = <region name> ex. us-east-1

[profile <child profile>]
role_arn = arn:aws:iam::<account_id_number>:role/<role_name>
output = json
region = <region name> ex. us-east-1
source_profile = <aws username>
```

***Changes to ~/.aws/credentials***
```
[default]

[<child profile>]  #Child profile is yours to name!
aws_access_key_id = <access key>
aws_secret_access_key = <secret key>

[<aws username>]
aws_access_key_id = <access key>
aws_secret_access_key = <secret key>
```

### AWS EC2 Prerequistes

- Create a keypair and download it from AWS EC2, you will only be able to download this keypair once.
- Move your keypair to ```~/.ssh```
- Change permissions of the keypair to 400. As follows:
```chmod 400 ~/.ssh/<keypair name>.pem```

### Running the Code

1. Clone the repo.

    ```git clone```

2. Source the script to assume aws role (needs to be done only if you do not have admin access to your aws accout and you are using boto which is required by some depriciated modules of ansible, eg: ec2 module).

    ```source ./script.sh```

    (The temporary credentials assigned, will time out after 1 hour of running the script. It will need to be run again after an hour)

3. Change the variables in inventory/example/all/vars.yml

4. Make sure that your aws private key is present (###.pem) in the ~/.ssh directory.

5. Run the master playbook

    ```ansible-playbook site-playbook.yml```
    <br>(to run it with default values)

    ```ansible-playbook site-playbook.yml -i inventory/example```
    <br>(to run it with the values updated in step 3)

Note: The master playbook as well as all the roles are idempotent.

![alt text](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Resources/Images/Playbooksuccess.png)

## Project Overview

The project has 5 working ansible roles:

1. **Visual Server**
2. **Web Server**
3. **Metricbeat & Httpd**
4. **Elasticsearch**
5. **Kibana**

### Functionality of each role:-

1. **Visual Server:**<br>
    It has the following 4 taks defined under it:
    - Create a security group with the required permisions.
    - Provisions a amazon linux 2 machine from aws of t2.medium tier and sets up ssh access into the machine.
    -  Adds the newly provisioned instance to a host group - visualserver.
    - Waits for SSH to come up.

2. **Web Server:**<br>
    It has the following 4 taks defined under it:
    - Create a security group with the required permisions.
    - Provisions a amazon linux 2 machine from aws of t2.medium tier and sets up ssh access into the machine.
    -  Adds the newly provisioned instance to a host group - webserver.
    - Waits for SSH to come up.

3. **Elasticsearch:**<br>
    It has the following 8 tasks defined under it:
    - Adds elasticsearch's gpg key.
    - Adds a verified yum repository for elasticsearch using the template elasticsearch.repo.j2.
    - Installs Java 8 on the machine which is a pre-requisite to elasticsearch.
    - Uses yum package manager to install elasticsearch.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the required configurations.
    - Configures elasticsearch by replacing configuration file jvm.options with our template file jvm.options.j2 which contains all the required configurations.
    - Restarts elasticsearch.
    - Starts elasticsearch.

4. **Kibana:**<br>
    It has the following 7 tasks defined under it:
    - Adds kibana's gpg key.
    - Adds a verified yum repository for kiaban using the template kibana.repo.j2.
    - Installs Java 8 on the machine which is a pre-requisite to kibana.
    - Uses yum package manager to install kibana.
    - Configures kibana by replacing configuration file kibana.yml with our template file kibana.yml.j2 which contains all the required configurations.
    - Restarts kibana.
    - Starts kibana.

5. **Metricbeat & HTTPD:**<br>
    It has the following 8 tasks defined under it:
    - Installs the latest version of Metribeat and Apache HTTPD using yum package manager.
    - Configures Metribeat to monitor HTTPD.
    - Configures Metribeat to not monitor the system which it does by default.
    - Stops HTTPD service.
    - Stops Metricbeat service.
    - Starts HTTPD service.
    - Starts Metribeat service.