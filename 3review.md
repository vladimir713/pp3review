[Вопросы для собеседования ПП3]

# SPRING

+ [Spring Security](#Spring-security)






## Spring Security
Spring Security предоставляет широкие возможности для защиты приложения. Кроме стандартных настроек для аутентификации, авторизации и распределения ролей и маппинга доступных страниц, ссылок и т.п., предоставляет защиту от различных вариантов атак

Spring Security - это список фильтров в виде класса FilterChainProxy, интегрированного в контейнер сервлетов, и в котором есть поле List<SecurityFilterChain>. Каждый фильтр реализует какой-то механизм безопасности. Важна последовательность фильтров в цепочке.

![Image alt](https://github.com/Shell26/Java-Developer/raw/master/img/Spring8.png)

Когда мы добавляем аннотацию @EnableWebSecurity добавляется DelegatingFilterProxy, его задача заключается в том, чтобы вызвать цепочку фильтров (FilterChainProxy) из Spring Security. 

В Java-based конфигурации цепочка фильтров создается неявно.

Если мы хотим настроить свою цепочку фильтров, мы можем сделать это, создав класс, конфигурирующий наше Spring Security приложение, и имплементировав интерфейс WebSecurityConfigurerAdapter. В данном классе, мы можем переопределить метод:
```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   http
   .csrf().disable()
   .authorizeRequests();
}
```

Именно этот метод конфигурирует цепочку фильтров Spring Security и логика, указанная в этом методе, настроит цепочку фильтров.

__Основные классы и интерфейсы__

__SecurityContext__ - интерфейс, отражающий контекст безопасности для текущего потока. Является контейнером для объекта типа Authentication. (Аналог - ApplicationContext, в котором лежат бины).

По умолчанию на каждый поток создается один SecurityContext. SecurityContext-ы хранятся в SecurityContextHolder.

Имеет только два метода: getAuthentication() и setAuthentication(Authentication authentication).
__SecurityContextHolder__ - это место, где Spring Security хранит информацию о том, кто аутентифицирован. Класс, хранящий в ThreadLocal SecurityContext-ы для каждого потока, и содержащий статические методы для работы с SecurityContext-ами, а через них с текущим объектом Authentication, привязанным к нашему веб-запросу.
![Image alt](https://github.com/Shell26/Java-Developer/raw/master/img/Spring9.png)

__Authentication__ - объект, отражающий информацию о текущем пользователе и его привилегиях. Вся работа Spring Security будет заключаться в том, что различные фильтры и обработчики будут брать и класть объект Authentication для каждого посетителя. Кстати объект Authentication можно достать в Spring MVC контроллере командой SecurityContextHolder.getContext().getAuthentication(). Authentication имеет реализацию по умолчанию - класс UsernamePasswordAuthenticationToken, предназначенный для хранения логина, пароля и коллекции Authorities.