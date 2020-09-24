The purposes of this challenge is to deploy an airflow webserver that is hosted on the cloud. 
By the end of this challenge, you should be able to see your cloud hosted airflow webserver through your browser.
This deployment can be the foundation of many python and SQL challenges found in this repository. 
With this challenge completed, you can translate any script or SQL commands over to Airflow's framework for Airflow experience.


### Dependencies
0. Ubuntu instance with airflow installed through the ansible playbook shown in the main README

## Steps
0. After having downloaded the `de-devtools` repository, run 

`make local` in your `/home/ubuntu/de-devtools` location

1. Edit inbound rules in your instance's security group to accept 'All TCP' From your IP address

EC2 > Security Groups > Your_instance's_sec_group_launch-wizard-n > Edit Inbound rules

2. You can run the airflow webserver and scheduler in the background. To run airflow in the background use the following command and press enter a few times if your terminal refuses to move on.

`nohup airflow webserver &`

`nohup airflow scheduler &`


3. Visit your airflow webserver from your browser with the following address

    {Public IPv4 DNS}:8080
    
Public IPv4 DNS looks something like this:

    ec2-10-100-100-110.us-west-1.compute.amazonaws.com
    
So the address will look something like this:

    ec2-10-100-100-110.us-west-1.compute.amazonaws.com:8080
    

4. (note) Future enhancements, or CI/CD challenges, will include securing your webserver site with openssl, not just depending on IP address. 
And finally, a docker+Kubernetes deployment.

Loom video guide following Steps 2 - 4:
https://www.loom.com/share/25c5b77875dc46f6bd9033d08390d642
