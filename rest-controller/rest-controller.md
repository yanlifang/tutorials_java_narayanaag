# Step by Step REST Controller for spring boot with JPA

### Add dependencies in pom.xml
1. Add spring boot web project dependencies, this also brings in dependencies of embedded tomcat.
    ```
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
    ```
### Create REST controller
1. Write new Controller class with XXXController
2. Annotate the class with `@RestController`
   * `@RestController` - marks the class as controller and handle REST request.
   * Helps serialize the response object to HttpResponse.

### GET Method 
1. GET method handles READ data from API, this helps retries data for given paramters, params are taken as query params.
2. Annotate method with `@GetMapping`
3. `@GetMapping` marks the method as following GET HTTP method.
4. Add paramter `path` to mapping to specificy context path to invoke the end point 
5. Add parameter `produces` to indicate response that API produces.
6. ex.@GetMapping(path="/employees", produces = "application/json")

### POST Method 
1. POST method handles Write operaton, takes in data in body and help creates entity.
2. Annotate method with `@GetMapping`
3. `@PostMapping` marks the method as following POST HTTP method.
4. Add paramter `path` to mapping to specificy context path to invoke the end point 
5. Add parameter `produces` to indicate response that API produces JSON as response body.
6. Add parameter `consumes` to indicate response that API takes in JSON as input body.
7. ex.`@PostMapping(path="/employee", produces = "application/json",consumes = "application/json")`
8. on posting to `/employee` , with JSON in request body , gives back JSON in response body.
9. `ResponseEntity<Employee>(employee, HttpStatus.CREATED)` - indicates that `Employee` is returned with HttpStatus as 201 - for created.

### JPA 
1. JPA helps add persitance layer to spring boot application
2. enable JPA , add below dependencies to `pom.xml`
    ```
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-data-jpa</artifactId>
      </dependency>
    ```
3. Create class for Entity to represent table in database , its called entity.
4. Annotate with `@Entity`, then name of the class represents name of table.
5. Add atrributes/fields in class which are representation of columns in table, annotate each @Column annotation and parameters `name` indicates column name
6. Example below `@Column(name = "LOGIN_NAME")` - indicates `loginName` is mapped to `LOGIN_NAME` column in table.
   ```
    @Entity
    public class User {
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY, generator = "USER_ID_SEQ")
      @SequenceGenerator(sequenceName = "user_seq", allocationSize = 1, name = "USER_ID_SEQ", initialValue = 4)
      Long id;

      @Column(name = "LOGIN_NAME")
      String loginName;
      
      public Long getId() {
        return id;
      }

      public void setId(Long id) {
          this.id = id;
      }

      public String getLoginName() {
          return loginName;
      }

      public void setLoginName(String loginName) {
          this.loginName = loginName;
      }
    }
   ```
 7. `@GeneratedValue` - indicate the field is generated and will not need to set and logic is from geneator with name `USER_ID_SEQ`
 8. `@SequenceGenerator(sequenceName = "user_seq", allocationSize = 1, name = "USER_ID_SEQ", initialValue = 4)` - logic to generate the id. uses `user_seq` sequence starting from intial value
 9. Add `XXXRepository` interface that extends `CrudRepository` with needed methods for queries and its definition.
    * `@Query` - helps define query for operations to database.
    ```
    public interface UserRepositary extends CrudRepository<User, Long> {

        // custom query example and return a stream
        @Query("select u from User u where u.email = :email")
        List<User> findByEmail(@Param("email") String email);

        @Query("select u from User u where u.loginName = :loginName")
        User findByLogin(@Param("loginName") String loginName);

        @Query("select u from User u where u.id = :id")
        User findById(@Param("id") String id);

        List<User> findAll();
    }
    ```
 10. Add `XXXRepository` class defined in #9 as field to controller class defined in #1
 11. Add Annotation @Inject to leverage spring's Dependency Injection inject into controller class.
 12. Add logic to GET/POST method defined to invoke operations on `XXXRepository`
 13. `save` method in `XXXRepository` - insert into DB
 14. `findAll` - finds all data in table.
   
### Compile 
Package the application using maven build command mvn clean package.
### Start the server using IDE.
### Make request to serve using end points defined
1. GET : `http://localhost:8080/employees` - get data from database
2. POST : `http://localhost:8080/employee` - write data to database 
