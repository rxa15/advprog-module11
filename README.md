# Module 11: Deployment on Kubernetes
## Tutorial Advanced Programming 2023/2024 Genap

* Nama  : Tengku Laras Malahayati
* NPM   : 2206081641
* Kelas : A

## Reflection
### Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?

Here is the logs comparation before and after I exposed it as a Service:

**Before**
![Logs Before Exposing As A Service](/images/Logs-Before-Exposing-as-Service.png)

**After**
![Logs After Exposing As A Service](/images/Logs-After-Exposing-as-Service.png)

Before configuring the application as a service, the logging mechanism only included entries when I accessed both the HTTP and UDP servers on my local port. However, after configuring it as a service and exposing the port to external server, accessing the service resulted in additional log entries indicating `GET /`, as the requests were now routed through the external server. This behavior persisted even after multiple attempts to access the application.

### Notice that there are two versions of `kubectl get` invocation during this tutorial section.The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
> Hint: Do some reading about [Namespace in Kubernetes
documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces
)

In Kubernetes, the `-n` option specifies the namespace for a given operation. It indicates that the operation should occur within the specified namespace. This option enables us to focus on specific namespaces when interacting with Kubernetes resources or running commands.

When executing a command with the `-n` option set to `kube-system`, it displays resources from the namespace containing core components of the Kubernetes system, hence it does not display the list of pods/services that users have created explicitly. Without using the `-n` option, resources explicitly created by users will be displayed instead.

### What is the difference between Rolling Update and Recreate deployment strategy?
> Hint: Read the Deployments documentation

The key difference between Rolling Update and Recreate Deployment strategies is the application's downtime during updates. With Recreate Deployment, all pods from the previous version are terminated before new pods with the latest version are created which results in application unavailability or downtime during this process. On the other hand, Rolling Update minimizes downtime by updating the application in stages, thus ensuring it remains available throughout. New pods are created one by one while the old pods are still operational so that the application to continue running during the update.

### Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt

Here are the procedures:
![Recreate deployment strategy-1](/images/Recreate-2.png)
![Recreate deployment strategy-1](/images/Recreate-3.png)

This is when I accessed the endpoint:
![Recreate deployment strategy-1](/images/Recreate-1.png)

### Prepare different manifest files for executing Recreate deployment strategy

To implement the Recreate deployment strategy, I created a new manifest file called `recreate_deployment.yaml`. I implemented the same method for creating `deployment.yaml` file and modified the `strategy` section to use `Recreate`, as shown in the following code:

```yaml
...
  strategy:
    type: Recreate
...
```

### What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster

Using Kubernetes manifest files offers significant advantages by providing a more structured approach to application deployment. Compared to doing it manually, manifest files help maintain configuration consistency across different environments. Additionally, the deployment process becomes simpler and more efficient. 

By using the `kubectl apply -f` command, deployments are faster and easier. With configuration declarations in manifest files, we can focus more on application development while Kubernetes automatically manages the infrastructure behind the scenes.
