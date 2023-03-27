# Constructor-based-Dependency-Injection
In this Document showing Constructor-based Dependency Injection in XML format and Annotation Format

# XML format
 we have a Car class that has two dependencies: an Engine and a Transmission. We declare a constructor that takes these dependencies as parameters.

```java
<!-- Car.java -->
public class Car {
    private Engine engine;
    private Transmission transmission;

    public Car(Engine engine, Transmission transmission) {
        this.engine = engine;
        this.transmission = transmission;
    }

    public void drive() {
        engine.start();
        transmission.shiftGear();
        System.out.println("Car is driving...");
    }
}

<!-- Engine.java -->
public class Engine {
    public void start() {
        System.out.println("Engine is started.");
    }
}

<!-- Transmission.java -->
public class Transmission {
    public void shiftGear() {
        System.out.println("Transmission is shifted.");
    }
}
```

We can then define our beans and their dependencies in an XML configuration file.
like this:

configuration class
```java
<!-- applicationContext.xml -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="engine" class="com.example.Engine" />
    <bean id="transmission" class="com.example.Transmission" />
    <bean id="car" class="com.example.Car">
        <constructor-arg ref="engine" />
        <constructor-arg ref="transmission" />
    </bean>

</beans>
```
In this XML configuration file, we define three beans: an Engine, a Transmission, and a Car. We define the Engine and Transmission beans as usual, and we also define the Car bean by calling its constructor with the Engine and Transmission beans as arguments using the <constructor-arg> tag.


Then, Mian Class in these we can use the ApplicationContext to load the XML configuration and retrieve the Car bean:
```java
// Main.java
public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Car car = (Car) context.getBean("car");
        car.drive();
    }
}
```
In the Main class, we use the ApplicationContext to load the XML configuration and retrieve the Car bean. We then call the drive() method on the Car bean, which will output the result.


> **_Note_**  The only difference in two classes that is main class and the configuration class.

# annotation

Clases 

```java
public class Car {
    private Engine engine;
    private Transmission transmission;

    public Car(Engine engine, Transmission transmission) {
        this.engine = engine;
        this.transmission = transmission;
    }

    public void drive() {
        engine.start();
        transmission.shiftGear();
        System.out.println("Car is driving...");
    }
}

public class Engine {
    public void start() {
        System.out.println("Engine is started.");
    }
}

public class Transmission {
    public void shiftGear() {
        System.out.println("Transmission is shifted.");
    }
}
```


Configuration Class
```java
@Configuration
public class AppConfig {
    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Transmission transmission() {
        return new Transmission();
    }

    @Bean
    public Car car() {
        return new Car(engine(), transmission());
    }
}
```

Main Class
```java
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    @Bean
    public CommandLineRunner run(Car car) {
        return args -> {
            car.drive();
        };
    }
}
```

> **_Note_**  1. When we go through this document the theory is mentioned in xml example same applicable for the annotation example.

> 2. for ther understanding of main diffrance is compare only Main Class and Configuration Class of xml example and annotation example.

