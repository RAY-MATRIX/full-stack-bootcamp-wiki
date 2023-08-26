# 25 Aug 2023 - Tutorial

> 只有写个人demo数据库的时候才可以在数据库中直接操作表，否则任何商业化操作都尽可能或只能通过代码“记录”下来，也就是Data Migration.

- 记录方法通过在项目中和创建sql文件
    - 命名约定，例如： `V1__create_user_table`
    - 主键 - `primary key` - 用来确认唯一标识符
    - `CHAR` - 存储字符串数据类型 - 固定长度（比VARCHAR更快因为使用的存储空间更小）
    - `VARCHAR` - 存储字符串数据类型- 可变长度
    ```sql
    CREATE TABLE "user_info" (
        "id" BIGSERIAL PRIMARY KEY,
        "email" VARCHAR(255) UNIQUE NOT NULL,
        "name" VARCHAR(255) NOT NULL,
        "password" CHAR(64) NOT NULL,
        "created_time" TIMESTAMP WITH TIME ZONE NOT NULL,
        "updated_time" TIMESTAMP WITH TIME ZONE NOT NULL
    );
    ```
    > 如果对V1进行修改并重新运行的话则会(被flyway) 报错（违反规范）
    > 解决办法： 创建一个新的sql文件来进行更新 例如：`V2__change_email_name`
- Digging more about Flyways
    https://documentation.red-gate.com/fd
    
- OOP是需要深入了解的编程思想 Object-Oriented Programming
    
- CRUD操作(用ORM `Object-relational mapping`代替传统的SQL来实现)
    
1. 
    - 先创建一个Model class，其中用field涵盖所有创建数据库表的数据
    - `@Column(unique = true, nullable = false)`用来声明该字段对应数据库表中的相对应的值不为空`NOT NULL`
    > 其中一旦名字对应不上，则`@Column(name = "xxx")`提供了name可以用来指定
    
    > `@Getter` `@Setter` 用来自定义get set结构
    
    > `@Entity`需要写在Model class上面负责让ORM知道本Model是数据库的表来存储值的。 其中`@Id` `GeneratedValue(Stragy = GenerationType.IDENTITY)`写在主键上方，是用来确认`Primary Key`的。
    
    > `@CreationTimestamp` 和 `@UpdateTimestamp`两个注释用于自动创建和更新时间
    
2. 创建一个Repository接口并继承JpaRepository（其中有一系列的功能来支持增删改查）
    `UserRepository.java`
    ```java
    public interface UserRepository extends JpaRepository<UserInfo, Long> {
        Optional<UserInfo> findByEmail(String email);
    }
    ```
    
    在`UserService.java`中可以使用
    
    ```java
    public class UserService {
        // 此时这个对象可以用一些列的增删改查功能
        private final UserRepository userRepository;
        
        public UserGetDto createUser(UserPostDto userPostDto) {
        
            UserInfo userInfo = new UserInfo();
            userInfo.setEmail(user.PostDto.getEmail());
            userInfo.setName(userPostDto.getName());
            userInfo.setPassword(userPostDto.getPassword());
            
            UserInfo savedUser = userRepository.save(userInfo);
            
            // 其他CRUD功能比如：
            // Delete
            // userRepository.delete(savedUser);
            // userRepository.deleteById(Long id);
            // Find
            // userRepository.findAll();
            // userRepository.findById(Long id);            
            
            UserGetDto userGetDto = new userGetDto();
            userGetDto.setEmail(savedUser.getEmail());
            userGetDto.setName(savedUser.getEName());
            userGetDto.setId(savedUser.getId());
            userGetDto.setCreatedTime(savedUser.getCreatedTime());
            userGetDto.setUpdatedTime(savedUser.getUpdatedTime());
            
            return userGetDto;
        }
        
        public UserGetDto findUserById(long userId) {
        // 注意，如果不写.get()方法则会直接返回一个Optional类型
        // 晚一些会进行代码重构
            UserInfo savedInfo = userRepository.findById(userId).get();
            
            UserGetDto userGetDto = new userGetDto();
            userGetDto.setEmail(savedUser.getEmail());
            userGetDto.setName(savedUser.getEName());
            userGetDto.setId(savedUser.getId());
            userGetDto.setCreatedTime(savedUser.getCreatedTime());
            userGetDto.setUpdatedTime(savedUser.getUpdatedTime());
            
            return userGetDto;
        }
        
        public UserGetDto findUserByEmail(String email) {
            UserInfo savedInfo = userRepository.findByEmail(email).get();
            
            UserGetDto userGetDto = new userGetDto();
            userGetDto.setEmail(savedUser.getEmail());
            userGetDto.setName(savedUser.getEName());
            userGetDto.setId(savedUser.getId());
            userGetDto.setCreatedTime(savedUser.getCreatedTime());
            userGetDto.setUpdatedTime(savedUser.getUpdatedTime());
            
            return userGetDto;
        }
    
    }
    ```
    
    在`UserGetDto.java`中：
    ``` java
    @Getter
    @Setter
    public class UserGetDto {
        private long id;
        private String name;
        private String email;
        private OffsetDateTime createdTime;
        private OffsetDateTime updatedTime;
    }
    ```
    
    在`UserController.java`中
    ```java
    @RestController
    @RequestMapping("/users")
    @RequiredArgsController
    public class UserController {
        private final UserService userService;
        
        @PostMapping
        public UserGetDto createUser(@RequestBody UserPostDto userPostDto) {
            return userService.createUser(userPostDto);
        } 
        
        @GetMapping("/{userId}")
        public UserGetDto findUserById(@PathVariable long userId){
            return userService.findUserById(userId);
        }
        
        @GetMapping
        public UserGetDto findUserByEmail(@RequestParameter String email) {
            return userService.findUserByEmail(email);
        }
    }
    ```
    