**Setup Cluster**
`terraform apply`


**Setup Node Exporter on EC2**
"sudo useradd --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
tar xvfz node_exporter-1.2.2.linux-amd64.tar.gz
sudo cp node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
sudo nano /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

sudo ufw allow 9100/tcp
sudo systemctl restart ufw"

**Configure the EKS cluster in local**
aws eks --region <region>  update-kubeconfig --name <cluster-name>
e.g. aws eks --region us-east-2  update-kubeconfig --name udacity-cluster


**Create namespace**
    `kubectl create namespace monitoring`


**Create the Kubernetes secret which references the above yaml file**
    `kubectl create secret generic additional-scrape-configs --from-file=prometheus-additional=prometheus-additional.yaml --namespace monitoring`

**Install the monitoring stack in Kubernetes using helm**
    Expected command : kubectl apply -f "path\to\values.yaml" --namespace monitoring

**Problem faced**
Erorr while trying to install the monitoring stack in Kubernetes using helm while using values.yaml file from starter project

**Solution** 
Replace the content of values.yaml file from `https://raw.githubusercontent.com/prometheus-community/helm-charts/main/charts/kube-prometheus-stack/values.yaml`.

`helm install prometheus prometheus-community/kube-prometheus-stack -f values.yaml --namespace monitoring`

Port forward to open the Grafana dashboard:
    `kubectl port-forward service/prometheus-grafana 3000:80 --namespace=monitoring`

**Install the Blackbox Exporter**
 `helm install prometheus-blackbox prometheus-community/prometheus-blackbox-exporter -f blackbox-values.yaml --namespace monitoring`
 helm install prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter -f blackbox-values.yaml --namespace monitoring
