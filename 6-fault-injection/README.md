# FAULT INJECTION
---

 - Use fault injection to test the Bookinfo application microservices for resiliency.

---

## Prerequisites

 - Apply application version routing as described in section 5 by run:
 
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```

---

## Insert a fault injection

 - inject a 7s delay between the reviews:v2 and ratings microservices for user jason
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml
```

 - Log with user jason (empty password), You expect the Bookinfo home page to load without errors in approximately 7 seconds. However, there is a problem: the Reviews section displays an error message.

---

## Debug the issue

 - Open the Developer Tools menu in you web browser and open the network tab
 
 - Reload the /productpage web page. You will see that the page actually loads in about 6 seconds
 
 - You’ve found a bug. There are hard-coded timeouts in the microservices that have caused the reviews service to fail

```
As expected, the 7s delay you introduced doesn’t affect the reviews service because the timeout between the reviews and ratings service is hard-coded at 10s. However, there is also a hard-coded timeout between the productpage and the reviews service, coded as 3s + 1 retry for 6s total. As a result, the productpage call to reviews times out prematurely and throws an error after 6s.

Bugs like this can occur in typical enterprise applications where different teams develop different microservices independently. Istio’s fault injection rules help you identify such anomalies without impacting end users.
```

---

## Injecting an HTTP abort fault

 - Introduce an HTTP abort to the ratings microservices for the test user "jason"
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml
```
 
 - In this case, you expect the page to load immediately and display the Ratings service is currently unavailable message.

---

## Cleanup

 - Remove the application routing rules

```
kubectl delete -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
```
