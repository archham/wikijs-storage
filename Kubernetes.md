## UNDER CONSTRUCTION {#under_construction}

Notes:

``` bash
#Shutdown Workload

kubectl get deployment --all-namespaces | grep -v -e ^kube-system -e ^NAMESPACE | awk '{print "-n "$1" deployment/"$2}' | while read line ; do kubectl scale --replicas=0 $line ; done
kubectl get statefulset --all-namespaces | grep -v -e ^kube-system -e ^NAMESPACE | awk '{print "-n "$1" statefulset/"$2}' | while read line ; do kubectl scale --replicas=0 $line ; done

# Startup Workload

kubectl get deployment --all-namespaces | grep -v -e ^kube-system -e ^NAMESPACE | awk '{print "-n "$1" deployment/"$2}' | while read line ; do kubectl scale --replicas=1 $line ; done
kubectl get statefulset --all-namespaces | grep -v -e ^kube-system -e ^NAMESPACE | awk '{print "-n "$1" statefulset/"$2}' | while read line ; do kubectl scale --replicas=1 $line ; done
```
