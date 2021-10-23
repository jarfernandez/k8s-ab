# k8s-ab

Kubernetes application running Apache Benchmark using image [devth/alpine-bench](https://github.com/devth/alpine-bench).

## Deployment

To deploy the application in a Kubernetes cluster run the following commands:

```bash
kubectl create -f ab-config.yaml
kubectl create -f ab-yaml
```
## Configuration

The application configuration is set in the `ab-config` ConfigMap defined in the `ab-config.yaml` file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ab-config
data:
  concurrency: "100" # Number of multiple requests to make at a time
  requests: "50000"  # Number of requests to perform
  timeout: "2"       # Seconds to max. wait for each response. Default is 30 seconds.
  url: http://nginx/ # URL to which requests will be sent
```

If the configuration in the `ab-config.yaml` file is modified after the application is running, the following commands must be executed to apply the new configuration:

```bash
kubectl apply -f ab-config.yaml
kubectl rollout restart deploy/ab
```

This will restart the pod or pods running the application with the new configuration.

## View the results

To view the results of Apache Benchmark run the following command:

```bash
kubectl logs $(kubectl get pods -l app=ab --no-headers | awk '{ print $1 }' | head -n 1)
```

## License

Copyright (c) 2021 José Arturo Fernández Díaz. Distributed under the [MIT](https://opensource.org/licenses/MIT) license.

