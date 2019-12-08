# OBSERVABILITY
---

 - In this section we will see istio observability features

---

## Expose istio add-ons systems

 - Edit the services below to update the service type from ClusterIP to LoadBalancer
```
kubectl patch svc kiali -p '{"spec": {"type": "LoadBalancer"}}' -n istio-system
kubectl patch svc prometheus -p '{"spec": {"type": "LoadBalancer"}}' -n istio-system
kubectl patch svc tracing -p '{"spec": {"type": "LoadBalancer"}}' -n istio-system
kubectl patch svc grafana -p '{"spec": {"type": "LoadBalancer"}}' -n istio-system
```

 - Wait for all the load balancer to be created and retrieve the external ip of the services above
```
kubectl get services kiali prometheus tracing grafana -n istio-system
```

---

## Collecting new telemetry data

 - Add a metric and log stream that Istio will generate and collect automatically.
```
kubectl apply -f ../istio/samples/bookinfo/telemetry/metrics.yaml
```

 - Get the external IP of the ingress gateway
```
kubectl get service istio-ingressgateway -n istio-system
```

 - In a new terminal run the below in order to send a traffic to the mesh
```
while true; do curl -o /dev/null -s "<ingress-gateway-external-ip>/productpage"; done
```

---

## Inspect Grafana

 - Get the external IP of the grafana service
```
kubectl get svc grafana -n istio-system
```
 - Browse to grafana
```
http://<grafana-external-ip>:3000
```

---

## Inspect Prometheus 

 - Get the external IP of the prometheus service
```
kubectl get svc prometheus -n istio-system
```
 - Browse to prometheus
```
http://<prometheus-external-ip>:9090
```

 - Go to status/targets to check what it's been monitored

---

## Inspect Kiali

 - Create a username variable for kiali (admin)
```
KIALI_USERNAME=$(read -p 'Kiali Username: ' uval && echo -n $uval | base64)
```

 - Create a password variable for kiali (admin)
```
KIALI_PASSPHRASE=$(read -sp 'Kiali Passphrase: ' pval && echo -n $pval | base64)
```

 - Create a secret to store kiali credentials
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

 - Get the kiali pod name
```
kubectl get pods -n istio-system | grep kiali
```

 - Delete the kiali pod to recreate it
```
kubectl delete pod <pod-name> -n istio-system
```

 - Wait until the kiali pod is recreated
```
kubectl get pods -n istio-system -w
```

 - Get the external IP of the kiali service
```
kubectl get svc kiali -n istio-system
```

 - Browse to kiali
```
http://<kiali-external-ip>:20001/kiali/console
```

---

## Inspect Jaeger

 - Get the external IP of the tracing service
```
kubectl get svc tracing -n istio-system
```
 - Browse to jaeger
```
http://<tracing-external-ip>:80
```

---