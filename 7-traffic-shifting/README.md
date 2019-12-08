# TRAFFIC SHIFTING
---

 - In this section we will see how to shift the traffic

---

## Route all traffic to v1

 - Route all traffic to the v1 version of each microservice
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
```

- Notice that the reviews part of the page displays with no rating stars, no matter how many times you refresh.

---

## Transfer 50% of the traffic from reviews:v1 to reviews:v3 

 - Configure the VirtualService to split the traffic
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml
```

 - Refresh the /productpage in your browser and you now see red colored star ratings approximately 50% of the time
 
---

## Transfer 100% of the traffic to v3

 - Assuming you decide that the reviews:v3 microservice is stable, you can route 100% of the traffic to reviews:v3 by applying this virtual service
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-reviews-v3.yaml
```

 - Now when you refresh the /productpage you will always see book reviews with red colored star ratings for each review.

---

## Cleanup

```
kubectl delete -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
```

---