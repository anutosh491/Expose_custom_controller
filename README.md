# Expose_custom_controller
A Kubernetes custom controller built to expose our applications to the external world


### Pre-requisites
One would need basic understanding of these go packages to understand the code framed.
1) k8s.io/client-go
2) k8s.io/api
3) k8s.io/apimachinery


### Use Cases
Let's say we have a k8s cluster and we want to deploy an application `APP1`, through a deployment. Now if we want an external user to be able to access it, we would have to create a service and an ingress resource in order to enable the user to access the application APP1. So first we create a service followed by an ingress resource .This custom controller does the exact same thing automatically so that all we need to do is deploy our application and it would be exposed automatically.


### Basic Building Blocks behind writing the controller
1) The controller should be able to figure out or watch through the creation of deployments and then take appropriate actions. Hence we can use the watch verb or a better option would be informers (as watch would query the Kube-API server several times which would increase load of the server)
2) Once we have an informer , we need to register some functions like let's say `handle_Add`, `handle_Delete` and `handle_Update` to keep a track on any addition, deletion or updation of the resource
3) Once these functions are setup , all they really need to do is make an entry on a data structure (`queue`) that we are going to maintain as a part of our controller.
4) Lastly we maintain a function or a routine which would get objects from the queue and then execute all the bussiness logic what we want to perform, which in our case is exposing our deployment/application.
