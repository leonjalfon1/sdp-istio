# INTELLIGENT ROUTING
---

 - In this section we will see how to manage routing using istio

---

## Configure subsets

 - Before you can use Istio to control the Bookinfo version routing, you’ll need to define the available versions, called subsets, in destination rules.
```
kubectl apply -f ../istio/samples/bookinfo/networking/destination-rule-all.yaml
```

## Route all traffic to only one version (reviews:v1)

 - Create a VirtualService to route all traffic to reviews:v1
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
```

 - Try now to reload the page multiple times, and note how only version 1 of reviews is displayed each time

---

## Special Routing for Specific User

 - Create a VirtualService to route user named Jason to the service reviews:v2
```
kubectl apply -f ../istio/samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```

 - To test, click Sign in from the top right corner of the page, and login using jason as user name with a blank password.

---

## Cleanup (skip if you will continue with section 6)

 - Remove the application virtual services
```
kubectl delete -f ../istio/samples/bookinfo/networking/virtual-service-all-v1.yaml
```

---

## Notes

 - DestinationRule: defines policies that apply to traffic intended for a service after routing has occurred.

 - Subset: A subset of endpoints of a service (can be used for scenarios like A/B testing, or routing to a specific version of a service)

 - VirtualService: Configuration affecting traffic routing

---