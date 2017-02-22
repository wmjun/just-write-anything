# 디스패치 (Dispatch)



**어떤 메소드를 호출할 것인가를 결정하여 그것을 실행하는 과정**을 말한다.

디스패치에는  2가지가 있다.

- Static Dispatch
- Dynamic Dispatch



아래 코드에서 각각의 디스패치가 일어난다.



## 정적인 디스패치 (Static Dispatch)



```java
public class Dispatch{
  
  static class Service{
    void run(){
      System.out.println("run");
    }
    
    void run(String msg){
      System.out.println(msg);
    }
  }
  
  public static void main(String[] args){
    new Service().run();
  }
  
}
```



위 프로그램에서 main을 실행하게 되면 Service 클래스에 정의된 run 메소드 중 어느 것이 실행된다는 것은 런타임 시점이 아닌 컴파일 시점에서 알 수 있다.



## 동적인 디스패치 (Dynamic Dispatch)



```java
public class Dispatch{
  
  static abstract class Service{
    abstract void run();
  }
  
  static class MyService1 extends Service{
    @Overried
    void run(){
      System.out.println("1");
    }
  }
  
  static class MyService2 extends Service{
    @Overried
    void run(){
      System.out.println("2");
    }
  }
  
  public static void main(String[] args){
    MyService svc = new MyService1();
    svc.run();
  }
  
}
```



위 프로그램에서 main을 실행하게 되면 어떤 클래스의 run 메소드가 실행될지는 컴파일 시점에 알 수 없다.
그러나 실제 실행을 해보면 "1"이 출력될 것임은 자명하다. 이때 **다이나믹 디스패치**가 일어난다. 



_참고 : [토비의 봄 TV 1회 - 재사용성과 다이나믹 디스패치, 더블 디스패치](https://www.youtube.com/watch?v=s-tXAHub6vg)_

