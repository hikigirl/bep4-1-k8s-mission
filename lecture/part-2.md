# p-14653-1-mission: 쿠버네티스 기초

## Part 2. Pod 기초

### 0003: Pod 생성하기
- Pod란 K8s에서 만들 수 있는 최소 단위
- Docker container들을 담는 바구니
- K8s는 컨테이너를 직접 관리하지 않고 Pod 단위로 관리
- 특징
  - 1개 이상의 컨테이너를 포함
  - 같은 네트워크, 같은 스토리지 공유
  - Pod는 임시적, 삭제되면 데이터도 소멸

작업 1
```bash
# nginx Pod 생성
kubectl run nginx-pod --image=nginx:latest

# Pod 목록 확인
kubectl get pods

# Pod 상태 확인 (실시간)
kubectl get pods -w

# Pod 상세 정보
kubectl describe pod nginx-pod
```

작업 2
```bash
# Pod 내부 쉘 접속
kubectl exec -it nginx-pod --bash

# nginx 설정 파일 확인
cat /etc/nginx/nginx.conf

# 나가기
exit

# Pod 로그 확인
kubectl logs nginx-pod

# Pod 삭제
kubectl delete pod nginx-pod
```

### 0004: yaml 파일로 Pod 정의
작업 1: pod yaml 파일 작성
```bash
# yaml 파일 작성
vi nginx-pod.yaml
```
작업 2: 선언형 방식으로 Pod 관리
```bash
# Pod 생성
kubectl apply -f nginx-pod.yaml

# Pod 상태 확인 (-o wide로 더 많은 정보)
kubectl get pods -o wide

# Pod YAML 확인 (Kubernetes가 추가한 정보 포함)
kubectl get pod nginx-pod -o yaml

# Pod 삭제 (파일 기반)
kubectl delete -f nginx-pod.yaml

# 특정 라벨로 Pod 필터링
kubectl get pods -l app=nginx
kubectl get pods -l environment=dev
```

### 0005: multi-container pod
- 멀티 컨테이너 패턴
  - 사이드카: 메인 앱을 보조
  - 앰버서더: 외부 통신을 대리
  - 어댑터: 출력을 변환

작업1: 사이드카 패턴 pod 작성, nginx와 로그 수집기
- `multi-container-pod.yaml` 작성

작업2: multi container pod 실행
```bash
# Pod 생성
kubectl apply -f multi-container-pod.yaml

# Pod 상태 확인 - READY 열에 2/2 표시
kubectl get pods

# 특정 컨테이너 로그 확인
kubectl logs multi-container-pod -c log-sidecar
```
- 테스트해보기
``` bash
# 1. nginx에 접속 요청 생성 (로그 발생)
kubectl exec multi-container-pod -c main-app -- curl localhost

# 2. 사이드카에서 로그 확인
kubectl logs multi-container-pod -c log-sidecar

# 3. 파드 종료
kubectl delete -f multi-container-pod.yaml
```