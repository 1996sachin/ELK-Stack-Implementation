🚀 Basic ELK Stack Implementation on Kali Linux

This project outlines the setup and deployment of the ELK Stack (Elasticsearch, Logstash, and Kibana) on Kali Linux, enabling centralized logging, analysis, and visualization for cybersecurity environments.

🔧 Components:

    Elasticsearch – Storage and search engine for log data

    Logstash – Data collection and parsing pipeline

    Kibana – Web UI for data visualization

    Kali Linux – Debian-based OS for testing and deployment

🧰 Prerequisites

    Kali Linux (or any Debian-based system like Ubuntu)

    Java 11 or higher (required for Elasticsearch)


🛠️ Step-by-Step Setup Guide

✅ 1. Install Java (OpenJDK 11)

sudo apt update
sudo apt install openjdk-11-jdk

Verification
Java --version


✅ 2. Install Elasticsearch

wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.0-amd64.deb
sudo dpkg -i elasticsearch-7.10.0-amd64.deb

To enable the service

sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

To verify 

curl -X GET "localhost:9200/"


✅ 3. Install Logstash

wget https://artifacts.elastic.co/downloads/logstash/logstash-7.10.0-amd64.deb
sudo dpkg -i logstash-7.10.0-amd64.deb

To verify

sudo systemctl enable logstash
sudo systemctl start logstash


✅ 4. Install Kibana

wget https://artifacts.elastic.co/downloads/kibana/kibana-7.10.0-amd64.deb
sudo dpkg -i kibana-7.10.0-amd64.deb

To verify

sudo systemctl enable kibana
sudo systemctl start kibana

To access Web GUI:

 👉 http://localhost:5601


✅ 5. To Configure Logstash

Nano file 
sudo nano /etc/logstash/conf.d/logstash.conf

Configuration

input {
    stdin {}
}

filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
}

output {
    elasticsearch {
        hosts => ["localhost:9200"]
        index => "logstash-%{+YYYY.MM.dd}"
    }
    stdout { codec => rubydebug }
}


✅ 6. Test the ELK Stack

Start Logstash:

sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf


📊 Visualize Logs in Kibana

Step 1: Access Kibana

Visit: http://localhost:5601
Go to the Discover tab.

Step 2: Create an Index Pattern

    Go to Stack Management → Index Patterns

    Click Create index pattern

    Enter: logstash-*

    Choose @timestamp as the time field

    Click Create index pattern
    

Step 3: View Logs in Discover

    Navigate to the Discover tab

    Choose logstash-* index pattern

    Adjust the time filter (top-right) to include your test logs (e.g., Last 15 minutes)


Step 4: Create Visualizations

    Go to Visualize Library → Create Visualization

    Choose a chart type (Bar, Line, Pie, etc.)

    Select index: logstash-*

    Choose fields like http.response.status_code for visual breakdowns

    Click Save


Step 5: Build Dashboards

    Go to Dashboard → Create New Dashboard

    Add your visualizations

    Customize layout and save


🧪 Troubleshooting

Check service status:

sudo systemctl status elasticsearch
sudo systemctl status logstash
sudo systemctl status kibana


Check Elasticsearch logs:

sudo journalctl -u elasticsearch


Restart services if needed:

sudo systemctl restart elasticsearch
sudo systemctl restart logstash
sudo systemctl restart kibana

