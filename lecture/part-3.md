# p-14653-1-mission: 쿠버네티스 기초

## Part 3. Deployment와 ReplicaSet

### 0006: Deployment 생성하기
- 앞선 강의에서 배운 pod는 __일회용__ = pod가 죽으면 __자동으로 다시 생성되지 않음__
- Deployment를 사용하는 것의 장점
  - 자동 복구: Pod가 죽으면 새로 만듬
  - 스케일링: Pod 개수를 쉽게 조절
  - 롤링 업데이트: 무중단으로 새 버전 배포
  - 롤백: 문제 발생 시 이전 버전으로 복원

작업 1
- `nginx-deployment.yaml` 생성

작업 2: Deployment 생성 및 확인
```bash
# Deployment 생성
kubectl apply -f nginx-deployment.yaml

# Deployment 목록 확인
kubectl get deployments
kubectl get deploy

# ReplicaSet 확인 (자동 생성됨)
kubectl get replicasets
kubectl get rs

# Pod 확인 (3개 생성됨)
kubectl get pods -l app=nginx

# Pod 하나 삭제해보기
kubectl delete pod <pod-name>
kubectl delete pod nginx-deployment-7c9dddfb48-8vqcc

# 바로 확인 - 새 Pod가 생성됨!
kubectl get pods -l app=nginx
```

```bash
# Deployment 삭제 (관련 ReplicaSet, Pod도 자동 삭제)
kubectl delete -f nginx-deployment.yaml
# 또는
kubectl delete deployment nginx-deployment
```

### 0007: 스케일링
- 스케일링이란? 트래픽에 따라 서버 인스턴스를 조절하는 것
- 스케일링의 종류
  - 수평 스케일링(Horizontal): 인스턴스 **개수**를 늘림 => **쿠버네티스는 수평에 특화**
  - 수직 스케일링(Vertical): 인스턴스 **성능**을 높임

작업1: 수동 스케일링
```bash
# Deployment 생성
kubectl apply -f nginx-deployment.yaml

# replicas 수 변경 (3 → 5)
kubectl scale deployment nginx-deployment --replicas=5

# Pod 변화 확인 (실시간)
kubectl get pods -l app=nginx -w

# Deployment 상태 확인
kubectl get deployment nginx-deployment

# 스케일 다운
kubectl scale deployment nginx-deployment --replicas=2
```

작업2: yaml 수정으로 스케일링
```yaml
# nginx-deployment.yaml을 수정
spec:
  replicas: 4    # 3 → 4로 변경
```

```bash
# 변경 적용
kubectl apply -f nginx-deployment.yaml

# 확인
kubectl get deployment nginx-deployment
```

작업3: 자동 복구 테스트
```bash
# 현재 Pod 목록 확인
kubectl get pods -l app=nginx

# Pod 하나를 강제 삭제
kubectl delete pod <pod-name>
kubectl delete pod nginx-deployment-7c9dddfb48-7ljxk

# 즉시 확인 - 새 Pod가 자동 생성
kubectl get pods -l app=nginx
```

자원 정리
```bash
# Deployment 삭제 (관련 ReplicaSet, Pod도 자동 삭제)
kubectl delete -f nginx-deployment.yaml
# 또는
kubectl delete deployment nginx-deployment
```

### 0008: 롤링 업데이트
- 한번에 모든 파드를 교체하지 않고 하나씩 교체
- 장점
  - 무중단 배포: 항상 일부 pod가 서비스 중
  - 안전한 배포: 문제 발생 시 롤백 가능
  - 점진적 검증: 새 버전이 안정적인지 확인하면서 배포

작업 1: 이미지 버전 업데이트
```bash
# Deployment 생성
kubectl apply -f nginx-deployment.yaml

# nginx 버전 업데이트 (1.25 → 1.26)
kubectl set image deployment/nginx-deployment nginx=nginx:1.26

# 롤아웃 상태 확인 (실시간)
kubectl rollout status deployment/nginx-deployment

# ReplicaSet 확인
kubectl get rs

# 업데이트 이력 확인
kubectl rollout history deployment/nginx-deployment

# CHANGE-CAUSE를 기록하려면
kubectl annotate deployment/nginx-deployment kubernetes.io/change-cause="Update to nginx 1.26"
```

작업 2: 롤백 (Rollback)
```bash
# 이전 버전으로 롤백
kubectl rollout undo deployment/nginx-deployment

# 특정 리비전으로 롤백
kubectl rollout undo deployment/nginx-deployment --to-revision=2

# 롤백 확인
kubectl describe deployment nginx-deployment
```
자원 정리
```bash
# 이번 강의에서 사용한 Deployment 삭제
kubectl delete deployment nginx-deployment
```

***
#### [이전 페이지로](https://github.com/hikigirl/bep4-1-k8s-mission)
<!-- 
```bash

```
 -->