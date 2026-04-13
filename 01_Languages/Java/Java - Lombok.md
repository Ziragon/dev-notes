Lombok - [[Java - Index|Java]] библиотека, которая автоматически генерирует шаблонный код.
```kts
compileOnly(libs.lombok)  
annotationProcessor(libs.lombok)
```

---
## 1. Data-аннотации

1. **`@Getter`/`@Setter`**: генерация геттеров/сеттеров в классе
2. **`@ToString`**: генерация toString метода, возвращающего информацию об объекте в виде строки
3. **`@EqualsAndHashCode`**: методы equals (логическое равенство объектов) и hashCode (хеш-код объекта) 
4. **`@Data`**: генерация всего вышеперечисленного + `@RequiredArgsConstructor`
5. **`@Value`**: ставит final для класса (запрет наследования) и полей, генерация вышеперечисленного кроме `@Setter`, использование `@AllArgsConstructor` - **в большинстве случаев заменяется record'ом**

---
## 2. Constructor-генераторы

1. **`@NoArgsConstructor`**: конструктор без аргументов
2. **`@AllArgsConstructor`**: конструктор, включающий в себя все поля класса
3. **`@RequiredArgsConstructor`**: конструктор с обязательными полями (final или @NonNull)

---
## 3. `@Builder`-паттерн

Аннотация создает скрытый **статический вложенный класс Builder** для класса, над которым стоит `@Builder`:
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