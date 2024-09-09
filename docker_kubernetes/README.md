#### Docker Compose for an Nginx Server:

Logs are stored on the host machine in the ./nginx/logs folder to persist between container restarts.
The custom bridge network is created with the specified subnet 172.20.8.0/24 and the Nginx container is assigned a static IP of 172.20.8.7.
The restart: always ensures that the container will restart automatically in case of a failure.

### Output
![alt text](https://github.com/anjanpaul/wsd-coding-challenge/blob/main/docker_kubernetes/Screenshot%20from%202024-09-09%2019-18-15.png)




#### Kubernetes Command to Identify Pod Restart Reason
To check the reason for the pod restart in the "internal" project under the "production" namespace, you can use the following Kubernetes command:

```
kubectl describe pod <pod-name> -n production

```

#### Possible Reasons for Java-App Pod Restarts
Considering the resource quota provided for the Java app, the restarts might occur for the following reasons:

* #### Memory Limit Exceeded:
The pod's memory limit is set to 1500 MiB, and the application has an Xmx (maximum heap size) of 1000 MiB. This doesn't leave enough room for non-heap memory (e.g., native memory or metaspace), which could cause the application to exceed its memory limits and lead to OOM (Out of Memory) errors. This will trigger pod restarts.
* #### CPU Resource Throttling: 
The pod has a CPU limit of 2000 millicores (2 cores), but if the app demands more CPU, Kubernetes will throttle it. This could lead to performance degradation and potential restarts due to health checks failing or timeouts.

You can check for memory or CPU-related issues using:

```
kubectl describe pod java-app-7d9d44ccbf-lmvbc -n production

```
To resolve the issue of memory limit exceeded for the Java application, there are a few adjustments you can make in both the Java application's configuration and the Kubernetes resource limits.



Increase the Pod’s Memory Limit:
* If possible, increase the memory limit of the Kubernetes pod to give the application enough headroom for heap and non-heap memory. Since the Java app's heap is set to Xmx 1000M, you need to account for non-heap memory like Metaspace, threads, and native memory.

* You can adjust the memory limit to, for example, 2000Mi or 2500Mi.
Update the pod's resource specification:


```
resources:
  requests:
    memory: "1000Mi"
  limits:
    memory: "2500Mi"

```

#### Adjust Java Memory Settings (Xmx and Xms):

Tuning the -Xmx value can help balance heap memory usage and non-heap memory.
If increasing the pod’s memory limit isn't an option, reduce the -Xmx to ensure the pod doesn’t hit the limit. For example, lowering it to -Xmx 800M could allow for enough space for other memory needs.



