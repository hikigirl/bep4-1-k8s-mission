<!-- template -->
# p-14653-1-mission: 쿠버네티스 기초

## Part 7. 실전 애플리케이션 배포

### 0015: 전체 애플리케이션 배포 예제
```bash
sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission (main)
$ kubectl apply -f namespace.yaml
namespace/demo-app created

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission (main)
$ kubectl apply -f backend-deployment.yaml
deployment.apps/backend created
service/backend-service created

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission (main)
$ kubectl apply -f frontend-deployment.yaml
deployment.apps/frontend created
service/frontend-service created

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission (main)
$ cd /c/code/programmers/bep4-1-k8s-mission/k8s-demo

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl apply -f namespace.yaml
namespace/demo-app unchanged

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl apply -f frontend-deployment.yaml
deployment.apps/frontend unchanged
service/frontend-service unchanged

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl apply -f backend-deployment.yaml
deployment.apps/backend unchanged
service/backend-service unchanged

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl apply -f /c/code/programmers/bep4-1-k8s-mission/k8s-demo
deployment.apps/backend unchanged
service/backend-service unchanged
deployment.apps/frontend unchanged
service/frontend-service unchanged
namespace/demo-app unchanged

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl get all -n demo-app
NAME                            READY   STATUS    RESTARTS   AGE
pod/backend-746f6d7d4d-bd7kz    1/1     Running   0          2m32s
pod/backend-746f6d7d4d-wv7lj    1/1     Running   0          2m32s
pod/frontend-7d84cb49b6-l8psm   1/1     Running   0          2m26s
pod/frontend-7d84cb49b6-t8wnp   1/1     Running   0          2m26s

NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/backend-service    ClusterIP      10.109.73.9     <none>        8080/TCP       2m32s
service/frontend-service   LoadBalancer   10.100.128.57   localhost     80:31837/TCP   2m26s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backend    2/2     2            2           2m32s
deployment.apps/frontend   2/2     2            2           2m26s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/backend-746f6d7d4d    2         2         2       2m32s
replicaset.apps/frontend-7d84cb49b6   2         2         2       2m26s

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl exec -n demo-app -it \
  $(kubectl get pod -n demo-app -l app=frontend -o jsonpath='{.items[0].metadata.name}') \
  -- curl backend-service:8080
Hello from Backend!

sunmin-pc@SUNMIN-DESKTOP MINGW64 /c/code/programmers/bep4-1-k8s-mission/k8s-demo (main)
$ kubectl delete namespace demo-app
namespace "demo-app" deleted
```

### 0016: 리소스 정리

***
#### [이전 페이지로](https://github.com/hikigirl/bep4-1-k8s-mission)

<!-- 
```bash

```
 -->