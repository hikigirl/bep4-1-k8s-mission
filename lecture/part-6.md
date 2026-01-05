<!-- template -->
# p-14653-1-mission: 쿠버네티스 기초

## Part 6. 스토리지

### 0014: PersistentVolume과 PersistentVolumeClaim

작업 1: PersistentVolume 생성
- `pv-local.yaml` 생성

작업 2: PersistentVolumeClaim 생성
- `pvc-local.yaml` 생성

작업 3: Pod에서 PVC 사용
- `pod-with-pvc.yaml` 생성

작업 4: 스토리지 확인 및 테스트
```bash
# 순서대로 생성
kubectl apply -f pv-local.yaml
kubectl apply -f pvc-local.yaml
kubectl apply -f pod-with-pvc.yaml

# PV 상태 확인
kubectl get pv

# PVC 상태 확인
kubectl get pvc
```
- 데이터 영속성 테스트
```bash
# 1. Pod에 데이터 쓰기
kubectl exec pvc-pod -- sh -c "echo 'Hello K8s' > /usr/share/nginx/html/index.html"

# 2. 확인
kubectl exec pvc-pod -- cat /usr/share/nginx/html/index.html
# 출력: Hello K8s

# 3. Pod 삭제
kubectl delete pod pvc-pod

# 4. Pod 다시 생성
kubectl apply -f pod-with-pvc.yaml

# 5. 데이터 확인 - 여전히 존재!
kubectl exec pvc-pod -- cat /usr/share/nginx/html/index.html
# 출력: Hello K8s  ← 데이터 유지! 🎉
```


### 0015: 


***
#### [이전 페이지로](https://github.com/hikigirl/bep4-1-k8s-mission)

<!-- 
```bash

```
 -->