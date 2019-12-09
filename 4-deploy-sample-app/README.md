# INSTALL ISTIO
---

 - In this section we will deploy a demo application: Bookinfo
 
 <img alt="Image 1.1" src="demo-app.png"  width="75%" height="75%">
 
 - The Bookinfo application is broken into four separate microservices:

   - productpage: The productpage microservice calls the details and reviews microservices to populate the page.
   - details: The details microservice contains book information.
   - reviews: The reviews microservice contains book reviews. It also calls the ratings microservice.
   - ratings: The ratings microservice contains book ranking information that accompanies a book review.

 - There are 3 versions of the reviews microservice:

   - Version v1: doesn’t call the ratings service.
   - Version v2: calls the ratings service, and displays each rating as 1 to 5 black stars.
   - Version v3: calls the ratings service, and displays each rating as 1 to 5 red stars.

---

## Deploy the Bookinfo Application

 -  Deploy sample apps by manually injecting istio proxy:
```
kubectl apply -f <(istioctl kube-inject -f ../istio/samples/bookinfo/platform/kube/bookinfo.yaml)
```

 - Define the virtual service and ingress gateway:
```
kubectl apply -f ../istio/samples/bookinfo/networking/bookinfo-gateway.yaml
```

 - Get the DNS name of the ingress gateway
```
kubectl get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}' -n istio-system ; echo
```

 - Browse to the application (this may take a minute or two)
```
http://<ingress-gateway-dns>/productpage
```

 - Click reload multiple times to see how the layout and content of the reviews changes as differnt versions (v1, v2, v3)
 
---

## Notes

 - VirtualService: defines a set of traffic routing rules to apply when a host is addressed.

 - Gateway: describes a load balancer operating at the edge of the mesh receiving incoming or outgoing HTTP/TCP connections

 - Ingress Gateways: specify services that should be exposed outside the cluster (like ingress resources)
