# Step by Step Guice for spring

### Changes to POM.xml
1. Update parent to spring boot starer with artifcat `spring-boot-starter-parent`
   ```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.5</version>
    </parent>
   ```

2. Project Depenedencies `spring-boot-starter-web`
   
   ```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
   ```
3. Build plugin
   * Spring Boot support in Apache Maven. It allows you to package executable jar or war archives.
   * Run Spring Boot applications, generate build information 
   * Start Spring Boot application.
   
   ```
   <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
   ```
### Beans or Business Objects
1. Add annotation `@Component` to indicate its a bean managed by spring 
2. Registers bean to Application Context
3. `@Service` to indicate its a bean that has business logic
4. `@Repository` - catch persistence-specific exceptions and re-throw them as one of Spring’s unified unchecked exceptions.
   
### Bean congifuration
1. Create java file BeanConfig by adding annotation `@Configuration`
2. Add bean definition with annotation `@Bean` , paramters supported by Bean 
   * `name`  - name of bean
   * `initMethod` - method to be called for initializing incase required
   * `destroyMethod` - method to be called for destruction.
3. `@ComponentScan` 
   * annotation to specify the packages that we want to be scanned for Beans. 
   * without arguments tells Spring to scan the current package and all of its sub-packages

### Inject Beans into application
Bean injection ways
   1. Setter - define setters using standard java naming convention
   2. Contructor - define contructor
   3. field - add annotation `@Inject` on field
   
### Fetch Bean
1. Fetch beans using `ApplicationContext.getBean`

```
ApplicationContext context = new AnnotationConfigApplicationContext(TravelSystemConfig.class);
Vehicle vehicle = context.getBean("Car",Vehicle.class);
vehicle.accelerate(5);
```
