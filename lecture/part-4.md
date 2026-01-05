<!-- template -->
# p-14653-1-mission: 쿠버네티스 기초

## Part 4. Service

### 0009: ClusterIP Service
- 서비스가 필요한 이유
  - Pod는 일시적임 = 삭제되고 다시 생성되면(재시작 등) IP 주소가 바뀜
- 서비스의 역할
  - 고정된 접점 제공: pod가 바뀌어도 서비스명/IP는 유지
  - 로드 밸런싱: 여러 Pod에 트래픽 분산
  - 서비스 디스커버리: 이름으로 pod를 찾을 수 있다
- 서비스의 종류

타입|접근 범위|사용 사례
---|---|---
ClusterIP|클러스터 내부만|백엔드 서비스, DB
NodePort|외부에서 노드IP:포트로|개발/테스트 환경
LoadBalancer|외부 로드밸런서|클라우드 프로덕션
<br>
작업1: ClusterIP Service 생성
- `nginx-service-clusterip.yaml` 생성

작업2: Service 테스트
```bash
# Deployment가 없다면 먼저 생성
kubectl apply -f nginx-deployment.yaml
kubectl get pods -l app=nginx  # Running 상태 확인

# Service 생성
kubectl apply -f nginx-service-clusterip.yaml

# Service 확인
kubectl get services
# 또는 줄여서
kubectl get svc

# Service 상세 정보 (Endpoints 확인)
kubectl describe service nginx-service

# 클러스터 내부에서 테스트 (임시 Pod 사용)
kubectl run curl-test --image=curlimages/curl -it --rm --restart=Never -- curl nginx-service
```

실습 정리
```bash
# Service 삭제
kubectl delete -f nginx-service-clusterip.yaml
# 또는
kubectl delete service nginx-service
```

### 0010: NodePort Service
- ClusterIP는 클러스터 내부에서만 접근 가능
- 외부에서 접근하려면 NodePort를 사용
- 특징
  - 모든 노드의 특정 포트를 개방
  - 범위: 30000~32767
  - 어떤 노드로 접속해도 같은 서비스에 연결

작업1: NodePort Service 생성
- `nginx-service-nodeport.yaml` 생성

작업2: NodePort로 외부 접근
```bash
# Deployment가 없다면 먼저 생성
kubectl apply -f nginx-deployment.yaml
kubectl get pods -l app=nginx  # Running 상태 확인

# Service 생성
kubectl apply -f nginx-service-nodeport.yaml

# Service 확인
kubectl get service nginx-nodeport

# 터미널에서 테스트
curl http://localhost:30080
```

### 0011: LoadBalancer Service
- 로드밸런서: 클라우드 환경에서 외부 로드 밸런서를 자동으로 생성하는 서비스 타입
- NodePort vs. LoadBalancer

구분|NodePort|LoadBalancer
---|---|---
외부 IP|노드 IP 사용|전용 공인 IP 할당
포트|30000~32767|원하는 포트
로드밸런싱|클라이언트가 노드 선택|로드밸런서가 분산
사용 환경|개발/테스트|프로덕션

작업1: LoadBalancer Service 생성
- `nginx-service-lb.yaml` 생성

작업2: LoadBalancer 접근
```bash
# Service 생성
kubectl apply -f nginx-service-lb.yaml

# Service 확인 (EXTERNAL-IP 확인)
kubectl get service nginx-lb

# Docker Desktop에서는 localhost로 접근
# http://localhost:80
curl http://localhost

# 자세한 정보 확인
kubectl describe service nginx-lb
```
실습 정리
```bash
# LoadBalancer Service 삭제
kubectl delete -f nginx-service-lb.yaml
# 또는
kubectl delete service nginx-lb

# 모든 Service와 Deployment 한번에 삭제
kubectl delete service nginx-service nginx-nodeport nginx-lb
kubectl delete deployment nginx-deployment
```

***
#### [이전 페이지로](https://github.com/hikigirl/bep4-1-k8s-mission)

<!-- 
```bash

```
 -->