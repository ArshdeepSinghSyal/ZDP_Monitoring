# Monitoring & Analysis

## Project Objective:

Our goal was to monitor httpd using metricbeat and use elasticsearch and kibana to store and visualize the collected metric on a distributed environment over the aws cloud.

The following diagram shows the architecture of how this has been achieved:

- Metricbeat and apache httpd are installed and configured on the web server.

- Metricbeat is configured to monitor httpd.

- Elasticsearch and Kibana are installed and configured on the visual server.

- Metricbeat collects metrics by monitoring httpd and continuously sends them to elasticsearch in real-time.

- Elasticsearch stores metrics sent to it by metricbeat.

- Kibana fetches metrics from elasticseach and creates visualizations.

<br>
## Application of project:

- Kibana is used to create visualizations which can be imported into the client's kibana dashboard. This would allow the client to view informative visuals about the services being monitored.

- Client can also create new visualizations and add them to his kibana dashboard.

- The following visualizations were created using kibana:

The link to the [presentation](https://github.com/ArshdeepSinghSyal/Zaloni-Assignment-3/blob/features/add_roles_cleaning/Presentation/Resources/AmazonEc2.png).


## Running The Code

Prerequisites:
- Python 2.6.5
- Ansible 2.6.5

1. Clone the repo
 
    ```git clone```

2. Source the script to assume aws role (needs to be done only if you do not have admin access to your aws accout) 

    ```source ./script.sh```

    The temporary credentials assigned, will time out after 1 hour of running the script. It will need to be run again after an hour. 

3. Change the variables in inventory/example/all/vars.yml

4. Run the playbook

    ```ansible-playbook site-playbook.yml```
    <br>(to run it with default values)

    ```ansible-playbook site-playbook.yml -i inventory/example```
    <br>(to run it with the values updated in step 3)

Add screenshot of playbook success

## Project Overview

The project has 5 working ansible roles:

1. Visual Server
2. Web Server
3. Metricbeat & Httpd
4. Elasticsearch
5. Kibana

### Functionality of each role:-

1. Visual Server:<br>
    It has the following _ taks defined under it:
    - Provisions a amazon linux 2 machine from aws of t2.medium tier.
    - Sets up ssh access into the machine
    -  Adds the newly provisioned instance to a host group by the name: visualserver

2. Web Server:<br>
    It has the following _ taks defined under it:
    - Provisions a amazon linux 2 machine from aws of t2.medium tier.
    - Sets up ssh access into the machine
    -  Adds the newly provisioned instance to a host group by the name: webserver

3. Elasticsearch:<br>
    It has the following _ tasks defined under it:
    - Downloads a verified yum repository for elasticsearch. 
    -  Installs java on the machine which is a pre requisite to elasticsearch. 
    - Uses yum package manager to install elasticsearch.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Restarts elasticsearch

4. Kibana:<br>
    It has the following _ tasks defined under it:
    - Downloads a verified yum repository for elasticsearch. 
    -  Installs java on the machine which is a pre requisite to elasticsearch. 
    - Uses yum package manager to install elasticsearch.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Restarts elasticsearch


5. Metricbeat & HTTPD:<br>
    It has the following _ tasks defined under it:
    - Downloads a verified yum repository for elasticsearch. 
    -  Installs java on the machine which is a pre requisite to elasticsearch. 
    - Uses yum package manager to install elasticsearch.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Configures elasticsearch by replacing configuration file elasticsearch.yml with our template file elasticsearch.yml.j2 whoch contains all the correct configurations.
    - Restarts elasticsearch