---
layout: single
title: "[DesignPattern] Decorator"

categories:
- Singleton

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

## **데코레이터 패턴**

데코레이터 패턴은 동작을 포함하는 특수 래퍼(wrapper) 객체 안에 객체를 배치해서 객체에 새 동작을 추가할 수 있는 디자인 패턴입니다.

데코레이터 패턴을 구현하는 방법에는 여러 가지가 있습니다. 가장 일반적인 방법 중 하나는 래퍼 인터페이스를 정의하고 래퍼 클래스를 구현하는 것입니다. 그런 다음 래퍼 클래스를 사용하여 객체에 새 동작을 추가할 수 있습니다.

예를 들어 다음 코드는 데코레이터 패턴을 사용하여 로그를 추가하는 래퍼를 만드는 방법을 보여줍니다.

# Java code 예시

```java
interface Component {
  void operation();
}

class ConcreteComponent implements Component {
  @Override
  public void operation() {
    // 기본 작업을 수행합니다.
  }
}

class LoggerDecorator implements Component {
  private Component component;

  public LoggerDecorator(Component component) {
    this.component = component;
  }

  @Override
  public void operation() {
    // 로그를 추가합니다.
    component.operation();
  }
}

```

이 코드에서 LoggerDecorator 클래스는 Component 인터페이스를 구현합니다. 클래스에는 또한 래핑된 객체를 참조하는 속성이 있습니다.

이 코드를 사용하려면 먼저 ConcreteComponent 클래스의 인스턴스를 생성합니다. 그런 다음 LoggerDecorator 클래스의 인스턴스를 생성하고 ConcreteComponent 클래스의 인스턴스를 래핑합니다. 그런 다음 래퍼 객체의 operation() 메서드를 호출하여 기본 작업을 수행하고 로그를 추가할 수 있습니다.

데코레이터 패턴은 객체에 동적으로 기능을 추가해야 할 때 유용한 디자인 패턴입니다. 데코레이터 패턴을 사용하면 코드를 수정하지 않고도 객체에 새 기능을 추가할 수 있습니다.


