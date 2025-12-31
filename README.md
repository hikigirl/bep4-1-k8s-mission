# p-14653-1-mission

### 0001: 쿠버네티스 클러스터 준비

### 0002: kubectl 기본 명령어
``` Bash
# 노드 목록 확인
kubectl get nodes

# 노드 상세 정보
kubectl describe node docker-desktop

# 클러스터 정보
kubectl cluster-info

# 현재 컨텍스트 확인
kubectl config current-context

# 네임스페이스 목록
kubectl get namespaces
kubectl get ns

# 특정 네임스페이스의 모든 리소스
kubectl get all -n kube-system
```

---

### 0003: Pod 생성하기
- Pod란 K8s에서 만들 수 있는 최소 단위
- Docker container들을 담는 바구니
- K8s는 컨테이너를 직접 관리하지 않고 Pod 단위로 관리
- 특징
  - 1개 이상의 컨테이너를 포함
  - 같은 네트워크, 같은 스토리지 공유
  - Pod는 임시적, 삭제되면 데이터도 소멸

작업 1
``` Bash
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
``` bash
# Pod 내부 쉘 접속
kubectl exec -it nginx-pod -- bash

# nginx 설정 파일 확인
cat /etc/nginx/nginx.conf

# 나가기
exit

# Pod 로그 확인
kubectl logs nginx-pod

# Pod 삭제
kubectl delete pod nginx-pod
```