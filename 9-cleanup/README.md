# CLEANUP
---

- List existent resources
```
kubectl get all
```

 - Delete existent resources
```
kubectl delete all --all
```

 - Uninstall istio
```
helm delete istio-init --purge
```
```
kubectl delete namespace istio-system
```

 - List existent resources
```
kubectl get all
```