![Image](https://github.com/user-attachments/assets/c4318983-db7b-4768-a789-7c33a3fbf220)

# ElasticSearch + Spring 사용

## 1. yml 설정
- elasticsearch의 url 값을 넣어준다
    ```yml
    spring:
        elasticsearch:
            uris: <uri>

    logging:
        level:
            org.elasticsearch.client: TRACE
    ```
- 편의를 위해 로그도 추가

##  2. Repository 설정
- elasticsearch JPA를 만들어 준다
    ```java
    import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

    public interface UserDocumentRepository extends <UserDocument, String> {
    
    }
    ```

## 3. 사용
- 이후 기존 JPA를 사용하듯 사용하면 된다!
    ```java
    import org.springframework.data.domain.Page;
    import org.springframework.data.domain.PageRequest;
    import org.springframework.web.bind.annotation.*;

    @RestController
    @RequestMapping("users")
    public class UserController {
        private final UserDocumentRepository userDocumentRepository;

        public UserController(UserDocumentRepository userDocumentRepository) {
            this.userDocumentRepository = userDocumentRepository;
        }

        @PostMapping()
        public UserDocument createUser(@RequestBody UserCreateRequestDto userCreateRequestDto) {
            UserDocument user = new UserDocument(
                    userCreateRequestDto.getAge(),
                    userCreateRequestDto.getId(),
                    userCreateRequestDto.getIsActive(),
                    userCreateRequestDto.getName()
            );
            return userDocumentRepository.save(user);
        }

        @GetMapping()
        public Page<UserDocument> findUsers() {
            return userDocumentRepository.findAll(PageRequest.of(0, 5));
        }

        @GetMapping("/{id}")
        public UserDocument findUserById(@PathVariable String id) {
            return userDocumentRepository.findById(id)
                    .orElseThrow(() -> new RuntimeException("존재하지 않는 사용자"));
        }

        @PutMapping("/{id}")
        public UserDocument updateUser(@PathVariable String id, @RequestBody UserUpdateRequestDto userUpdateRequestDto) {
            UserDocument user = userDocumentRepository.findById(id)
                    .orElseThrow(() -> new RuntimeException("존재하지 않는 사용자"));
            user.setName(userUpdateRequestDto.getName());
            user.setAge(userUpdateRequestDto.getAge());
            user.setIsActive(userUpdateRequestDto.getIsActive());
            return userDocumentRepository.save(user);
        }

        @DeleteMapping("/{id}")
        public void deleteUser(@PathVariable String id) {
            UserDocument user = userDocumentRepository.findById(id)
                    .orElseThrow(() -> new RuntimeException("존재하지 않는 사용자"));
            userDocumentRepository.delete(user);
        }
    }
    ```