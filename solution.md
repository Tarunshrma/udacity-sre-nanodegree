Create namespace :
    `kubectl create namespace monitoring`


Create the Kubernetes secret which references the above yaml file:
    `kubectl create secret generic additional-scrape-configs --from-file=prometheus-additional=prometheus-additional.yaml --namespace monitoring`

Install the monitoring stack in Kubernetes using helm:
    Expected command : kubectl apply -f "path\to\values.yaml" --namespace monitoring

**Problem faced**
Erorr while trying to install the monitoring stack in Kubernetes using helm while using values.yaml file from starter project

**Solution** 
Replace the content of values.yaml file from `https://raw.githubusercontent.com/prometheus-community/helm-charts/main/charts/kube-prometheus-stack/values.yaml`.

helm install prometheus prometheus-community/kube-prometheus-stack -f values.yaml --namespace monitoring

Port forward to open the Grafana dashboard:
    `kubectl port-forward service/prometheus-grafana 3000:80 --namespace=monitoring`

**Install the Blackbox Exporter**
 `helm install prometheus-blackbox prometheus-community/prometheus-blackbox-exporter -f blackbox-values.yaml --namespace monitoring`
 helm install prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter -f blackbox-values.yaml --namespace monitoring
