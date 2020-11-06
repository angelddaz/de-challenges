1. Generate a .crt and private .key file for your airflow webserver with the following commands
    * cd && mkdir certs && cd certs
    * openssl req \
       -newkey rsa:2048 -nodes -keyout domain.key \
       -x509 -days 365 -out domain.crt
2. Update your `~/airflow/airflow.cfg` file to accept your SSL certificates and keys for the web server
    * web_server_ssl_cert = ~/certs/server.crt
    * web_server_ssl_key = ~/certs/server.key
3. Reboot your server and attempt to visit in your browser with `https://` in front of your public IPv4 DNS address, which would be followed by the port number
    * ex: htpps://ec2-1-1-1.us-east-1.amazon.com:8080

