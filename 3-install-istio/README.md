# INSTALL ISTIO
---

 - In this section we will install istio in our EKS cluster

---

## Install the Istio CLI

 -  Download istio CLI by run:
```
curl -L https://git.io/getLatestIstio | sh -
mv istio-* ../istio
sudo mv -v ../istio/bin/istioctl /usr/local/bin/
```

## Install Istio

 - Define service account for Tiller
```
kubectl apply -f ../istio/install/kubernetes/helm/helm-service-account.yaml
```

 - Install Istio CRD's
```
helm install ../istio/install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

 - Install Istio core components
```
helm install ../istio/install/kubernetes/helm/istio --name istio --namespace istio-system --set global.configValidation=false --set sidecarInjectorWebhook.enabled=false --set grafana.enabled=true --set servicegraph.enabled=true --set kiali.enabled=true --set "kiali.dashboard.jaegerURL=http://jaeger-query:16686" --set "kiali.dashboard.grafanaURL=http://grafana:3000" --set tracing.enabled=true
```

 - Verify the installation
```
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```
 
---
