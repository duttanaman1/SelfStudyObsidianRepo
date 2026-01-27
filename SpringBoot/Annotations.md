| Annotation                         | Description                                                                        |
| ---------------------------------- | ---------------------------------------------------------------------------------- |
| `@AliasFor`                        | Used inside custom annotations to declare aliases for attributes.                  |
| `@Async`                           | Executes a method asynchronously in a background thread (requires `@EnableAsync`). |
| `@Autowired`                       | Injects beans automatically by type.                                               |
| `@Bean`                            | Declares a bean inside a `@Configuration` class.                                   |
| `@CacheConfig`                     | Shared cache settings for a class (cache names, key generator, etc.).              |
| `@CacheEvict`                      | Removes entry/entries from cache when method executes.                             |
| `@CachePut`                        | Updates/refreshes cache with latest method return value.                           |
| `@Cacheable`                       | Marks a method whose result should be cached.                                      |
| `@Captor`                          | Creates an `ArgumentCaptor` in Spring tests with Mockito.                          |
| `@CollectionTable`                 | Specifies a table for collections of basic/embeddable types in JPA.                |
| `@Column`                          | Maps an entity attribute to a database column.                                     |
| `@Component`                       | Marks a class as a Spring-managed bean.                                            |
| `@ComponentScan`                   | Enables component scanning for `@Component`, `@Service`, etc.                      |
| `@Conditional`                     | Allows bean creation only if condition matches.                                    |
| `@ConditionalOnBean`               | Loads config if another bean exists.                                               |
| `@ConditionalOnClass`              | Loads config if a class exists on classpath.                                       |
| `@ConditionalOnCloudPlatform`      | Loads config only if running on specific cloud (AWS, Azure, etc.).                 |
| `@ConditionalOnExpression`         | Uses SpEL to conditionally load beans.                                             |
| `@ConditionalOnJava`               | Loads config depending on Java version.                                            |
| `@ConditionalOnJndi`               | Loads config if a specific JNDI location exists.                                   |
| `@ConditionalOnMissingBean`        | Loads config only if no bean of type exists.                                       |
| `@ConditionalOnMissingClass`       | Loads config if a class is absent.                                                 |
| `@ConditionalOnNotWebApplication`  | Loads config if app is NOT a web app.                                              |
| `@ConditionalOnProperty`           | Loads config if a property matches a value.                                        |
| `@ConditionalOnResource`           | Loads config if a resource exists.                                                 |
| `@ConditionalOnSingleCandidate`    | Loads bean if exactly one candidate exists.                                        |
| `@ConditionalOnWebApplication`     | Loads config only in a web app.                                                    |
| `@Configuration`                   | Declares a configuration class.                                                    |
| `@ConfigurationProperties`         | Maps external properties (`.yml`/`.properties`) to POJO.                           |
| `@ConfigurationPropertiesScan`     | Scans packages for `@ConfigurationProperties` classes.                             |
| `@ConstructorBinding`              | Binds properties via constructor (immutable config objects).                       |
| `@Controller`                      | Marks a web controller.                                                            |
| `@ControllerAdvice`                | Global exception handling, model attributes, etc.                                  |
| `@CookieValue`                     | Binds HTTP cookie value to method parameter.                                       |
| `@CrossOrigin`                     | Enables CORS support.                                                              |
| `@DataJpaTest`                     | Slice test annotation for JPA repositories.                                        |
| `@DeleteMapping`                   | Shortcut for `@RequestMapping(method=DELETE)`.                                     |
| `@DependsOn`                       | Declares bean initialization order.                                                |
| `@Document`                        | Marks a class as a MongoDB document.                                               |
| `@DynamicInsert`                   | JPA Hibernate: only non-null fields included in INSERT SQL.                        |
| `@DynamicUpdate`                   | JPA Hibernate: only modified fields included in UPDATE SQL.                        |
| `@ElementCollection`               | Defines a collection of basic/embeddable types in JPA.                             |
| `@Embedded`                        | Defines an embeddable JPA object.                                                  |
| `@EmbeddedId`                      | Composite key embedded in JPA entity.                                              |
| `@EnableAsync`                     | Enables asynchronous execution (`@Async`).                                         |
| `@EnableAutoConfiguration`         | Enables Spring Boot auto-configuration.                                            |
| `@EnableBatchProcessing`           | Enables Spring Batch features.                                                     |
| `@EnableCaching`                   | Enables Spring caching abstraction.                                                |
| `@EnableConfigurationProperties`   | Enables support for `@ConfigurationProperties`.                                    |
| `@EnableDiscoveryClient`           | Enables service registration with discovery service (Eureka, Consul, etc.).        |
| `@EnableFeignClients`              | Enables Feign clients in microservices.                                            |
| `@EnableJpaRepositories`           | Enables Spring Data JPA repositories.                                              |
| `@EnableMongoRepositories`         | Enables Spring Data MongoDB repositories.                                          |
| `@EnableNeo4jRepositories`         | Enables Spring Data Neo4j repositories.                                            |
| `@EnableReactiveMongoRepositories` | Enables reactive MongoDB repositories.                                             |
| `@EnableScheduling`                | Enables scheduling support (`@Scheduled`).                                         |
| `@EnableTransactionManagement`     | Enables declarative transactions.                                                  |
| `@Entity`                          | Marks a JPA entity.                                                                |
| `@EntityListeners`                 | Hooks entity lifecycle callbacks.                                                  |
| `@Enumerated`                      | Maps enums to DB (ORDINAL/STRING).                                                 |
| `@ExceptionHandler`                | Handles exceptions in controllers.                                                 |
| `@Filter`                          | Hibernate filter mapping.                                                          |
| `@GeneratedValue`                  | Auto-generates primary key values.                                                 |
| `@GetMapping`                      | Shortcut for GET requests.                                                         |
| `@Id`                              | Marks primary key field in JPA.                                                    |
| `@IdClass`                         | Defines composite key with a separate ID class.                                    |
| `@Import`                          | Imports config classes.                                                            |
| `@ImportResource`                  | Imports XML-based bean definitions.                                                |
| `@Index`                           | Defines DB index on column/field.                                                  |
| `@Inheritance`                     | Defines inheritance mapping strategy in JPA.                                       |
| `@InitBinder`                      | Binds/initializes web request params to controller method.                         |
| `@IntegrationTest`                 | Bootstraps integration tests.                                                      |
| `@JsonBackReference`               | Handles bidirectional JSON serialization.                                          |
| `@JsonCreator`                     | Marks constructor/factory method for JSON deserialization.                         |
| `@JsonFormat`                      | Defines date/number format for JSON fields.                                        |
| `@JsonIgnore`                      | Excludes field from JSON serialization.                                            |
| `@JsonIgnoreProperties`            | Ignore multiple properties in JSON.                                                |
| `@JsonInclude`                     | Include/exclude fields based on null/empty.                                        |
| `@JsonManagedReference`            | Counterpart of `@JsonBackReference`.                                               |
| `@JsonProperty`                    | Explicitly binds JSON field name to property.                                      |
| `@JsonRootName`                    | Wraps JSON with a root name.                                                       |
| `@JsonView`                        | Serializes JSON according to a defined view.                                       |
| `@JoinColumn`                      | Defines foreign key column.                                                        |
| `@JoinTable`                       | Defines join table for many-to-many relationships.                                 |
| `@Lazy`                            | Delays bean initialization.                                                        |
| `@Lob`                             | Marks a field as a Large Object (BLOB/CLOB).                                       |
| `@LocalServerPort`                 | Injects assigned port in test.                                                     |
| `@Lookup`                          | Resolves bean lookup method dynamically.                                           |
| `@ManyToMany`                      | Defines many-to-many relationship in JPA.                                          |
| `@ManyToOne`                       | Defines many-to-one relationship.                                                  |
| `@MapperScan`                      | Scans for MyBatis mappers.                                                         |
| `@Max`                             | Validation: maximum allowed value.                                                 |
| `@Min`                             | Validation: minimum allowed value.                                                 |
| `@ModelAttribute`                  | Binds model attributes to handler method.                                          |
| `@NamedEntityGraph`                | Defines named entity graphs for fetch plans.                                       |
| `@NotBlank`                        | Validation: must not be blank.                                                     |
| `@NotEmpty`                        | Validation: must not be empty.                                                     |
| `@NotNull`                         | Validation: must not be null.                                                      |
| `@Nullable`                        | Indicates a nullable field.                                                        |
| `@Order`                           | Defines order of execution.                                                        |
| `@Param`                           | Used in Spring Data queries for named parameters.                                  |
| `@PathVariable`                    | Binds URI variable to method param.                                                |
| `@PersistenceContext`              | Injects JPA `EntityManager`.                                                       |
| `@PostConstruct`                   | Runs after bean creation.                                                          |
| `@PreAuthorize`                    | Spring Security: authorizes method before execution.                               |
| `@PreDestroy`                      | Runs before bean destruction.                                                      |
| `@Primary`                         | Marks preferred bean when multiple exist.                                          |
| `@Profile`                         | Activates beans for specific profiles.                                             |
| `@PropertySource`                  | Loads external properties file.                                                    |
| `@PutMapping`                      | Shortcut for PUT request.                                                          |
| `@Qualifier`                       | Resolves bean ambiguity.                                                           |
| `@Query`                           | Defines custom JPQL/SQL query in Spring Data.                                      |
| `@Repository`                      | Marks a DAO and enables exception translation.                                     |
| `@RepositoryRestResource`          | Exposes Spring Data repository as REST resource.                                   |
| `@RequestAttribute`                | Binds request attribute to method parameter.                                       |
| `@RequestBody`                     | Binds HTTP body to parameter.                                                      |
| `@RequestHeader`                   | Binds HTTP header to parameter.                                                    |
| `@RequestMapping`                  | Maps HTTP requests to handler methods.                                             |
| `@RequestParam`                    | Binds query parameters.                                                            |
| `@RequestPart`                     | Binds multipart request (file upload).                                             |
| `@ResponseBody`                    | Serializes return value to response body.                                          |
| `@ResponseStatus`                  | Sets HTTP response status for a method.                                            |
| `@RestClientTest`                  | Test slice for REST clients.                                                       |
| `@RestController`                  | Combines `@Controller` + `@ResponseBody`.                                          |
| `@RestControllerAdvice`            | Combines `@ControllerAdvice` + `@ResponseBody`.                                    |
| `@Retryable`                       | Enables retry logic on method failures.                                            |
| `@Scheduled`                       | Declares scheduled task (cron, fixed rate).                                        |
| `@Scope`                           | Defines bean scope.                                                                |
| `@Service`                         | Marks service layer bean.                                                          |
| `@SessionAttributes`               | Stores model attributes in session.                                                |
| `@SessionScope`                    | Defines session-scoped bean.                                                       |
| `@SpringBootApplication`           | Combines `@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan`.           |
| `@Sql`                             | Executes SQL during tests.                                                         |
| `@SqlConfig`                       | Configures `@Sql` behavior.                                                        |
| `@SqlGroup`                        | Groups multiple `@Sql` annotations.                                                |
| `@Table`                           | Maps entity to database table.                                                     |
| `@Temporal`                        | Maps `Date`/`Calendar` to SQL temporal type.                                       |
| `@TestConfiguration`               | Specialized config for test context.                                               |
| `@Transactional`                   | Declares transaction boundaries.                                                   |
| `@TransitionalEventListener`       | Transactional event listener.                                                      |
| `@Valid`                           | Triggers validation.                                                               |
| `@Validated`                       | Variant for groups validation.                                                     |
| `@Value`                           | Injects property values.                                                           |
| `@Version`                         | Marks optimistic lock version column.                                              |
| `@WebMvcTest`                      | Slice test for Spring MVC.                                                         |
| `@WebFluxTest`                     | Slice test for WebFlux controllers.                                                |