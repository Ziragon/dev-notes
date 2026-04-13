Lombok - [[Java - Index|Java]] библиотека, которая автоматически генерирует шаблонный код при компиляции в `.class` файлы. Пример внедрения в `build.gradle.kts`:
```kts
compileOnly(libs.lombok)  
annotationProcessor(libs.lombok)
```

---
## 1. Data-аннотации

1. **`@Getter`/`@Setter`**: генерация геттеров/сеттеров в классе
	- В параметрах можно указать уровень доступа к методу (`PUBLIC`, `PROTECTED` и т.д.)
2. **`@ToString`**: генерация toString метода, возвращающего информацию об объекте в виде строки
	- `includeFieldNames`
3. **`@EqualsAndHashCode`**: методы equals (логическое равенство объектов) и hashCode (хеш-код объекта) 
4. **`@Data`**: генерация всего вышеперечисленного + `@RequiredArgsConstructor`
5. **`@Value`**: ставит final для класса (запрет наследования) и полей, генерация вышеперечисленного кроме `@Setter`, использование `@AllArgsConstructor` - **в большинстве случаев заменяется record'ом**
6. **`@With`**: позволяет создать методы `withField`, позволяющие создать копию объекта с одним измененным полем

---
## 2. Constructor-генераторы

1. **`@NoArgsConstructor`**: конструктор без аргументов
2. **`@AllArgsConstructor`**: конструктор, включающий в себя все поля класса
3. **`@RequiredArgsConstructor`**: конструктор с обязательными полями (final или @NonNull)

---
## 3. `@Builder`-паттерн

Аннотация создает скрытый **статический вложенный класс Builder** для класса, над которым стоит `@Builder`, используется как замена конструктора:
```java
@Builder
@Getter
public class User { 
	private final Long id; 
	private final String name; 
	private final String email; 
	private final int age; 
}
```

Вызов билдера для экземпляра класса:
```java
User myUser = User.builder() 
	.id(1L) 
	.name("Ivan")
	.email("ivan@example.com") 
	.age(25)
	.build();
```

1. **`@Builder.Default`**: позволяет добавить полю класса значение по умолчанию, которое будет использоваться, если в билдере не указан сеттер для поля
	```java
	@Builder.Default
	private String status = "NEW";
	```

2. **`@Singular`**: позволяет автоматически при билде создавать пустую коллекцию и добавлять в нее элементы через билдер поэлементно:
	```java
	// Вместо .tags(List.of("java", "spring")) 
	User.builder() 
		.tag("java") // Первый элемент
		.tag("spring") // Второй
		.build();
	```

---
## 4. Логирование

1. **`@Slf4j`**: создает статическое финальное поле логгера внутри класса, использующее интерфейс **Slf4j* **(Simple Logging Facade for Java)**, добавляет в класс строку и позволяет использовать методы логгера (`log.info`, `log.debug`):
	```java
	private static final org.slf4j.Logger log = 
		org.slf4j.LoggerFactory.getLogger(YourClass.class);
	```

2. **`@XSlf4j`**: создает расширенный Slf4J логгер с методами `entry()` и `exit()`. Нужен для более глубокой отладки и трассировки потока выполнения программы

---
## 5. Прочее

1. **`@SneakyThrows`**: заставляет компилятор игнорировать [[Java - Exceptions|проверяемые исключения]] без необходимости указывания `throws`. 