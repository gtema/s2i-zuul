# s2i-zuul

This project aims to gather s2i builder images for the zuul@openshift

**NOTE:** Running in a minishift requires following change each time minishift starts.
Otherwise scheduler is not able to reach itself through the service
```
[localhost]$ minikube ssh
[minishift]$ sudo ip link set docker0 promisc on
```
