<!-- template -->
# p-14653-1-mission: 쿠버네티스 기초

## Part 5. ConfigMap과 Secret

### 0012: ConfigMap 사용하기
- 애플리케이션 설정을 ConfigMap으로 관리한다.
- 사용 시 장점
  - 설정과 코드 분리: 이미지 빌드 불필요
  - 환경별 설정 가능: dev, staging, prod 등 다르게 적용
  - 중앙 관리: 한 곳에서 설정 관리

작업1: ConfigMap 생성
- `app-configmap.yaml` 생성

작업2: ConfigMap을 Pod에서 사용
1. 방법 1: 모든 설정을 환경변수로 주입
   1. `pod-with-configmap.yaml` 생성
2. 방법 2: 특정 키만 환경변수로 주입
3. 방법 3: 파일로 마운트

작업3: ConfigMap 관리
```bash
# ConfigMap 생성
kubectl apply -f app-configmap.yaml

# Pod 생성
kubectl apply -f pod-with-configmap.yaml

# 환경변수 확인
kubectl exec app-pod -- env | grep APP

# ConfigMap 내용 확인
kubectl get configmap app-config -o yaml

# ConfigMap 수정
kubectl edit configmap app-config

#실습 정리
# ConfigMap과 Pod 삭제
kubectl delete -f pod-with-configmap.yaml
kubectl delete -f app-configmap.yaml
# 또는
kubectl delete pod app-pod
kubectl delete configmap app-config
```

### 0013: Secret 사용하기
Secret vs. ConfigMap

구분|ConfigMap|Secret
---|---|---
용도|일반 설정|민감정보
저장방식|평문|base64 인코딩
메모리 저장|디스크|tmpfs(메모리, 더 안전)

작업1: secret 생성
```bash
# 명령어로 Secret 생성
kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=secretpassword123
# Secret 확인 (base64 인코딩됨)
kubectl get secret db-secret -o yaml
# base64 디코딩해서 확인
kubectl get secret db-secret -o jsonpath='{.data.username}' | base64 -d
```
작업3: Secret을 Pod에서 사용

***
#### [이전 페이지로](https://github.com/hikigirl/bep4-1-k8s-mission)

<!-- 
```bash

```
 -->