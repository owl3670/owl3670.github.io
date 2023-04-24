---
layout: single
title: "[Kubernetes] StatefulSet"

categories:
- Kubernetes

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

Kubernetes StatefultSet 은 Stateful 한 application 의 상태를 관리하는 데 사용하는 컨트롤러로 deployment 와는 다르게 pod 의 고유성과 순서를 보장하며, 각 pod 이 고유한 볼륨을 가질 수 있도록 한다.

# StatefulSet 사용처

StatefulSet 은 다음과 같은 상황에서 유용하게 사용 할 수 있다.

- Unique 한 Network 식별자가 필요한 경우
- 영속적인 저장소가 필요한 경우
- 자동화된 rolling update 가 필요한 경우

# StatefulSet 제약 사항

- StatefulSet 을 다운시키더라도 volume 은 삭제되지 않음
- 각 pod 의 식별자를 사용한 요청 및 응답을 하기 위해서는 Headless Service 가 필요함
- StatefulSet 이 삭제 되더라도 pod 들의 제거를 보장하지는 않음 (StatefulSet 의 prior 를 0 으로 조정하는 방향으로 pod 제거를 해야함)

# Manifest 예시

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  serviceName: my-service
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image
        volumeMounts:
        - name: data
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

# StatefulSet 배포 전략

## RollingUpdate

StatefulSet 에서 spec.updateStrategy.type 에서 rollingUpdate (default 이다) 로 설정되면 update 가 발생할 시 큰 색인 순서부터 작은 순서 순으로 pod 이 종료되었다 재시작 되며, pod 은 1개씩 작업하게 된다.

## PartitionRollingUpdate

spec.updateStrategy.rollingUpdate.partition 을 명시해서 rolling update 를 partitioning 할 수 있으며, 이를 확용해서 Canary 배포의 Roll out 혹은 단계적 Roll out 을 수행할 수 있다.

## Unavailable Pod

Kubernetes v1.24 부터 spec.updateStrategy.rollingUpdate.maxUnavailable 을 명시해서 사용 불가능한 pod 을 최대 몇 개까지 허용할 것인지 조절할 수 있다.

## 강제 rollback

RollingUpdate 를 사용할 경우 직접 수동으로 복구를 해야 할 수도 있는데, Pod 의 Template 이 정상적이지 않아 Running 및 Ready 상태가 되지 않도록 했다면 Template 복구 뿐만 아닌 비정상적으로 켜진 pod 을 수동으로 삭제하여야 한다.