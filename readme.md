kubectl create namespace logging

kubectl create -f kubernetes/elastic.yaml -n logging


kubectl get pods -n logging
kubectl get service -n logging

kubectl port-forward elasticsearch-0 9200:9200 -n logging


kubectl patch svc "kibana" --namespace "logging" --patch '{"spec": {"type": "LoadBalancer"}}'




SERVICE_IP=$(kubectl get svc "kibana" --namespace "logging"  --output jsonpath='{.status.loadBalancer.ingress[0].ip}')

ELASTIC_URL="http://${SERVICE_IP}:5601"

https://portworx.com/run-ha-elasticsearch-elk-google-kubernetes-engine/
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
https://mherman.org/blog/logging-in-kubernetes-with-elasticsearch-Kibana-fluentd/