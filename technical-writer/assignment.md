# Debug Operations in Kubernetes

This article describes common commands that can be used to debug your Kubernetes clusters.

You can use the kubectl Command Line Interface (CLI) to interact with Kubernetes and query the state of your cluster resources.

When you issue commands using kubectl, the requests are sent to the Kubernetes API server to retrieve information about your cluster resources.

**Note:** These commands are standard across all Kubernetes environments (regardless of cloud provider or provisioner).

## List Pods in a Namespace

Use the following command to list all pods in a namespace including their status:

```shell
kubectl get pods --namespace <namespace-name>
```

Replace `<namespace-name>` with your chosen namespace (for example: `k8s-app`). If the namespace is not included, your default namespace is used.

Example output:

```shell
NAME     READY   STATUS    RESTARTS   AGE
pod-01   1/1     Running   0          2d
pod-02   1/1     Running   0          1d
pod-03   1/1     Running   0          5d
pod-04   1/1     Running   1          3d
```

## View Logs for a Pod

Use the following command to retrieve logs for a specific pod in a namespace:

```shell
kubectl logs <pod-name> --namespace <namespace-name>
```

Replace `<pod-name>` and `<namespace-name>` with your chosen pod and namespace (for example: `pod-04` and `k8s-app`).

Example output:

```shell
2023-08-29T10:15:30.123Z INFO  Server started on port 9001
2023-08-29T10:15:31.456Z INFO  Connected to database successfully
2023-08-29T10:16:05.789Z DEBUG User login attempt: userjosh
2023-08-29T10:16:06.012Z INFO  User userjosh logged in successfully
2023-08-29T10:17:12.345Z WARN  High CPU usage detected: 85%
2023-08-29T10:18:00.678Z ERROR Failed to connect to external API: timeout
2023-08-29T10:18:01.901Z INFO  Retrying API connection...
2023-08-29T10:18:03.234Z INFO  Successfully connected to external API
```

### Explore Log Output using `less`

If you use the `logs` command in the example above, the information is outputted to your terminal.

Alternatively, use the following command to explore the logs using `less`:

```shell
kubectl logs <pod-name> --namespace <namespace-name> | less
```

## Open a Terminal Session with a Pod

Use the following command to open a terminal session with a Pod:

```shell
kubectl exec --it <pod-name> --namespace <namespace-name> -- /bin/bash
```

Replace `<pod-name>` and `<namespace-name>` with your chosen pod and namespace (for example: `pod-04` and `k8s-app`).

If the command is successful, the terminal session starts inside the pod.

Use the command line to access logs and files contained within the pod.

## Clone and Debug a Pod

Use the following command to clone a pod for debugging purposes:

```shell
kubectl debug <pod-name> --namespace <namespace-name> -it --image=busybox --copy-to=<clone-name>
```

Replace `<pod-name>`, `<namespace-name>`, and `<clone-name>` with your chosen pod, namespace, and clone name (for example: `pod-04`, `k8s-app`, and `debug-pod-04`).

If the command is successful, a terminal session opens inside the cloned pod.

You can now interact with this pod without affecting the original and the session will not terminate if an error occurs inside the container.

## References

- [What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/)
- [Set your default namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#setting-the-namespace-preference)
- [View the kubectl command reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-)
