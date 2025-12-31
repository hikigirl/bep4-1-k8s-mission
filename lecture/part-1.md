# p-14653-1-mission: 쿠버네티스 기초

## Part 1. 세팅 및 기본 명령어

### 0001: 쿠버네티스 클러스터 준비
- Docker Desktop에서 쿠버네티스 사용 설정 필요

### 0002: kubectl 기본 명령어
```bash
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

**참고: 자주 쓰는 명령어**

명령어|의미
---|---
get|목록 조회
describe|상세 정보
create|생성
delete|삭제
apply|생성/수정
logs|로그 확인
exec|실행