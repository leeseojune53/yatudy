# 서비스 로케이터(SL)란?

### 📌 정의

애플리케이션에 필요할 수 있는 **모든 서비스를 얻는 방법을 알고있는 객체**를 갖는 것이다.

**IoC**를 **구현**하는 DI 외 방법 중 하나.

### 🔗 종속성 다이어그램

![img](https://ichi.pro/assets/images/max/724/1*EfAFtp0lApHhPuQdUfCMDw.png)

#### 실제 사용방법

```java
class ServiceLocator{
    private static ServiceLocator sInstance;
    public static void load(ServiceLocator arg) {
        sInstance = arg;
    }
    private Map services = new HashMap();
    public static Object getService(String key){
        return sInstance.services.get(key);
    }
    public void loadService (String key, Object service) {
        services.put(key, service);
    }
}

class Loader {
   private void configure() {
        ServiceLocator locator = new ServiceLocator();
        locator.loadService("LocalBookFinder", new LocalBookFinder());
        locator.loadService("RemoteBookFinder", new RemoteBookFinder());      
        ServiceLocator.load(locator);
    }
}

class Library{
	BookFinder finder = (BookFinder) ServiceLocator
        .getService("LocalBookFinder");
}
```

위 코드를 보면 ServiceLocator에 Loader가 Service를 추가해주고, Library class에서 그 Service를 injection해서 사용한다.

### 👍 다른 방법들..

IoC를 구현하는 다른 방법은 아래와 같이 있다.

- Events
- Delegates

등 등