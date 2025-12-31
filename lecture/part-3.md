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