---
layout: single
title: "[DesignPattern] Singleton"

categories:
- Singleton

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

## **싱글톤 패턴**

싱글톤 패턴은 클래스에 단 하나의 인스턴스만 생성되도록 보장하는 디자인 패턴입니다. 싱글톤 패턴을 사용하면 객체의 공유 인스턴스를 만들 수 있으므로 특정 작업에 사용할 수 있습니다. 예를 들어, 네트워크 연결이나 데이터베이스 연결과 같은 리소스가 한 개만 있는 경우 싱글톤 패턴을 사용하여 객체의 공유 인스턴스를 만들 수 있습니다.

싱글톤 패턴을 구현하는 방법에는 여러 가지가 있습니다. 가장 일반적인 방법 중 하나는 생성자를 private으로 선언하고 클래스에서 정적 인스턴스를 생성하는 것입니다. 그런 다음 클래스에서 정적 메서드를 제공하여 정적 인스턴스를 반환할 수 있습니다.

# Java code 예시

```java
class NetworkConnection {
  private NetworkConnection() {}

  private static NetworkConnection instance = new NetworkConnection();

  public static NetworkConnection getInstance() {
    return instance;
  }

  public void connect() {
    // 네트워크에 연결합니다.
  }

  public void disconnect() {
    // 네트워크에서 연결을 끊습니다.
  }
}

```

이 코드에서 NetworkConnection 클래스는 private 생성자와 정적 인스턴스를 갖습니다. 클래스에는 또한 정적 메서드가 제공되어 정적 인스턴스를 반환합니다.

이 코드를 사용하려면 먼저 NetworkConnection 클래스의 getInstance() 메서드를 호출하여 정적 인스턴스를 가져옵니다. 그런 다음 연결을 설정하고 관리하는 데 정적 인스턴스를 사용할 수 있습니다.

싱글톤 패턴은 특정 작업에 사용할 수 있는 공유 객체를 만들고 싶을 때 유용한 디자인 패턴입니다. 싱글톤 패턴을 사용하면 객체의 여러 인스턴스를 생성할 필요가 없으므로 메모리와 성능을 절약할 수 있습니다.

