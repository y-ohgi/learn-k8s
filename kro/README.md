## Install Kro

```sh
export KRO_VERSION=$(curl -sL \
    https://api.github.com/repos/awslabs/kro/releases/latest | \
    jq -r '.tag_name | ltrimstr("v")'
  )
```

```sh
echo $KRO_VERSION
```

```sh
helm install kro oci://public.ecr.aws/kro/kro \
  --namespace kro \
  --create-namespace \
  --version=${KRO_VERSION}
```

```sh
$ helm -n kro list
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
kro     kro             1               2024-11-15 20:22:11.251301 +0900 JST    deployed        kro-0.1.0       0.1.0
```

## Create ResourceGroup

```sh
kubectl apply -f kro/resourcegroup.yaml
```

## Create Applications

```sh
kubectl apply -f kro/nginx.yaml
kubectl apply -f kro/httpd.yaml
```

```sh
kubectl get applications

NAME                STATE    SYNCED   AGE
nginx-application   ACTIVE   True     74s
httpd-application   ACTIVE   True     78s
```

```sh
kubectl get deploy,svc

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-app   2/2     2            2           25s
deployment.apps/httpd-app   3/3     3            3           29s

NAME                        TYPE        CLUSTER-IP        EXTERNAL-IP   PORT(S)   AGE
service/kubernetes          ClusterIP   192.168.194.129   <none>        443/TCP   52m
service/nginx-app-service   ClusterIP   192.168.194.147   <none>        80/TCP    22s
service/httpd-app-service   ClusterIP   192.168.194.209   <none>        80/TCP    20s
```

## Uninstall

```sh
kubectl delete applications nginx-application httpd-application
```

```sh
kubectl delete resourcegroup my-application
```

```sh
helm uninstall kro -n kro
```
