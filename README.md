# karpenter-demo

## 개요  
Cluster Autoscaler는 AWS Auto Scaling을 사용해서 특정 시간에 노드를 늘리고 줄일 수 있습니다.  
하지만 Karpenter를 사용하면 Auto Scaling Group을 사용하지 않기 때문에 해당 기능을 사용하지 못합니다. 

## 해결 방법
1. KEDA Cron을 사용해서 특정 시간이 되면 오버 프로비저닝 pod를 늘리고 싶은 노드의 개수만큼 셋팅합니다.
2. 오버 프로비저닝 pod에는 파드안티어피니티가 설정되어있습니다.
3. cron에 설정한 시간이 되면 파드가 늘어나고 파드안피티니때문에 늘어난 파드의 개수만큼 노드가 늘어나게 됩니다.

## KEDA 설치
```
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
helm install keda kedacore/keda -n keda --create-namespace
```

## ScaledObject cron 설정
```
kubectl apply -f keda/keda-over-provioning.yaml
```

## 오버프로비저닝 pod 배포
```
kubectl apply -f keda/nginx.yaml
```

## Karpenter Provisioner 설치
```
kubectl apply -f ops-provisioner.yaml
kubectl apply -f a10-provisioner.yaml
```
