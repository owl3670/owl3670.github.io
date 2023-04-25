---
layout: single
title: "[Kubernetes] ConfigMap"

categories:
- Kubernetes

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

# ConfigMap

ConfigMap은 컨테이너 이미지와 분리된 설정을 제공하는 Kubernetes 오브젝트이다. ConfigMap은 컨테이너 이미지를 빌드할 때 설정을 포함하지 않고, 컨테이너가 실행될 때 설정을 주입할 수 있도록 한다.

## ConfigMap 사용처

ConfigMap은 다음과 같은 상황에서 유용하게 사용할 수 있다.

- 컨테이너 이미지를 빌드할 때 설정을 포함하지 않고, 컨테이너가 실행될 때 설정을 주입할 수 있도록 한다.
- 설정을 변경할 때마다 컨테이너 이미지를 다시 빌드할 필요 없이, 설정만 변경할 수 있다.
- 설정을 컨테이너 이미지와 분리하여, 설정을 변경할 때마다 컨테이너 이미지를 다시 빌드할 필요 없이, 설정만 변경할 수 있다.

## ConfigMap 생성

ConfigMap은 다음과 같이 생성할 수 있다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
data:
    example.property.1: value1
    example.property.2: value2
```

## ConfigMap 사용

ConfigMap은 다음과 같이 사용할 수 있다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
    containers:
    - name: example-container
        image: example-image
        env:
        - name: EXAMPLE_PROPERTY_1
            valueFrom:
                configMapKeyRef:
                name: example-configmap
                key: example.property.1
        - name: EXAMPLE_PROPERTY_2
            valueFrom:
                configMapKeyRef:
                name: example-configmap
                key: example.property.2
```
