There are two purposes of this challenge 
1. deploy a jupyter notebook webserver on your Ubuntu instance
2. make the webserver's UI accessible to your IP address with password credentials


Follow the Ubuntu shell instructions in the block starting and ending with three back ticks.

```
# install OS level dependencies for cloud code editor inside of ubuntu shell
pip3 install notebook
sudo apt-get install ipython3
sudo apt install jupyter-notebook

## create jupyter pw inside of ipython3 shell
ipython3
from IPython.lib import passwd
passwd()
Enter password: [Create password and press enter] Verify password: [Press enter]
'sha1:98ff0e580111:12798c72623a6eecd54b51c006b1050f0ac1a62d' # Copy this whole string
## above string will be used in line 58
## exit ipython3 shell
exit 

# Advanced Notebook Config in /home/ubuntu working directory
sudo jupyter-notebook --generate-config
mkdir certs
cd certs

## enter blank for all questions in the following command
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem


sudo vim ~/.jupyter/jupyter_notebook_config.py

# insert the following block of code at the beginning of the file
    
    c = get_config()

    # Kernel config
    c.IPKernelApp.pylab = 'inline'  # if you want plotting support always in your notebook

    # Notebook config
    c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem' #location of your certificate file
    c.NotebookApp.ip = '*'
    c.NotebookApp.open_browser = False  #so that the ipython notebook does not opens up a browser by default
    c.NotebookApp.password = u'sha1:98ff0e580111:12798c72623a6eecd54b51c006b1050f0ac1a62d'  #the encrypted password we generated above in line 34
    # Set the port to 8888, the port we set up in the AWS EC2 set-up
    c.NotebookApp.port = 8888

# final ubuntu shell command

sudo chown $USER:$USER /home/ubuntu/certs/mycert.pem

# After finishing configuration of jupyter
# allow all traffic from your IP address by going to 'NETWORK & SECURITY' > 'Security Groups'
# inside of aws console

# Visit https://{aws_host_name}:{port number}
# and proceed with password created in line 34.

# You may now write prototype data pipelines in the cloud
```

