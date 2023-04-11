---
layout: single
title: "[Spring] Graceful Shutdown"

categories:
- Spring

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

# Graceful Shutdown

서버를 종료할 때, 무작정 종료하면 안된다. 서버가 종료되는 동안에도 클라이언트의 요청을 처리해야 하기 때문이다. 이를 위해 스프링에서는 `Graceful Shutdown` 이라는 기능을 제공한다.

# Graceful Shutdown 이란?

`Graceful Shutdown` 은 서버가 종료되는 동안에도 클라이언트의 요청을 처리할 수 있도록 하는 기능이다. 이를 위해 스프링은 `ServerContainer` 라는 인터페이스를 제공한다. 이 인터페이스를 구현한 클래스는 `ServerContainer` 를 통해 서버가 종료되는 시점을 알 수 있다.

# ServerContainer

`ServerContainer` 는 `ServletContainerInitializer` 를 구현한 클래스이다. 이 클래스는 `ServletContainerInitializer` 를 구현한 클래스를 찾아서 `onStartup` 메소드를 호출한다. 이 때, `ServletContainerInitializer` 를 구현한 클래스는 `@HandlesTypes` 어노테이션을 통해 `ServerContainer` 를 구현한 클래스를 찾을 수 있다.

```java
@HandlesTypes(ServerContainer.class)
public class SpringServletContainerInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> webAppInitializerClasses, ServletContext servletContext) throws ServletException {
        // ...
    }
}
```

`ServerContainer` 는 `ServletContextListener` 를 구현한 클래스이다. 이 클래스는 `ServletContext` 가 생성되는 시점에 `contextInitialized` 메소드를 호출한다. 이 때, `ServletContext` 는 `ServerContainer` 를 구현한 클래스의 인스턴스를 `ServletContext` 에 저장한다.

```java
public class ServerContainerInitializer implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        ServletContext servletContext = sce.getServletContext();
        servletContext.setAttribute(ServerContainer.class.getName(), this);
    }
}
```

`ServerContainer` 는 `ServletContext` 에 저장된 인스턴스를 통해 서버가 종료되는 시점을 알 수 있다.

```java
public class ServerContainer implements ServletContextListener {
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        // ...
    }
}
```

# Graceful Shutdown 구현

`ServerContainer` 를 구현한 클래스를 만들고, `ServletContext` 에 저장한다.

```java
public class GracefulShutdown implements ServerContainer {
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        // ...
    }
}
```

`ServerContainer` 를 구현한 클래스를 `ServletContainerInitializer` 에 등록한다.

```java
@HandlesTypes(ServerContainer.class)
public class SpringServletContainerInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> webAppInitializerClasses, ServletContext servletContext) throws ServletException {
        // ...
        servletContext.addListener(new GracefulShutdown());
    }
}
```
