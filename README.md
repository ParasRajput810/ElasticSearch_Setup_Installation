# Elk Installation 

- We need to use a ubuntu 20.04 ami of t2.medium.

### Update the ubuntu instance and install nginx and java. 

- `sudo apt update`

- `sudo apt install openjdk-8-jdk`

- `sudo apt-get install -y nginx`

- `sudo systemctl enable nginx`

### Install ElasticSearch

- ` sudo wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.2.0-amd64.deb`

- `sudo dpkg -i elasticsearch-7.2.0-amd64.deb`

### Install Kibana

- `sudo wget https://artifacts.elastic.co/downloads/kibana/kibana-7.2.0-amd64.deb`

- `sudo dpkg -i kibana-7.2.0-amd64.deb`

### Install Logstash

-  `sudo wget https://artifacts.elastic.co/downloads/logstash/logstash-7.2.0.deb`

- `sudo dpkg -i logstash-7.2.0.deb`

### Install Dependencies

- `sudo apt-get install -y apt-transport-https`

### Install FileBeat

- `wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.2.0-amd64.deb`

- `sudo dpkg -i filebeat-7.2.0-amd64.deb`

### Modify elasticsearch yaml file

- `sudo vi /etc/elasticsearch/elasticsearch.yml`

- **Make below changes in this file :-**

``` 
cluster.name: my-application
node.name: node-1
http.port: 9200
network.host: localhost
```

- `sudo systemctl start elasticsearch`

### Modify Kibana yaml file

- `sudo vi /etc/kibana/kibana.yml`

- **Make below changes in the file :-**

```
server.port: 5601
server.host: "localhost"
```

- `sudo systemctl start kibana`

- `sudo apt-get install -y apache2-utils`

- `sudo htpasswd -c /etc/nginx/htpasswd.users kibadmin`

- `sudo vi /etc/nginx/sites-available/default`

```
    server {
        listen 80;
        server_name 3.108.42.168;
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/htpasswd.users;
        location / {
            proxy_pass http://localhost:5601;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
 }
}
```

- `sudo systemctl restart nginx`

### Accessing Kibana Dashboard

- Copy the Ip of the machine and paste the Ip on the Browser.