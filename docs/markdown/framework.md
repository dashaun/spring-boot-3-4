## Spring Framework 6.2

---

#### Baseline Upgrades

Spring Framework 6.2 raises its minimum requirements with the following libraries:

- For GraalVM native image support only, Hibernate 6.5
- FreeMarker 2.3.33
- HtmlUnit 4.2
- Codecs and converters now officially support Protobuf 4.x, raising our baseline to Protobuf 3.29.
- We also recommend an upgrade to Jackson 2.18 while preserving runtime compatibility with Jackson 2.15+ for the time being.

---

### Core Container

- Revised autowiring algorithm 
- Deeper generic type matching.
- Component scanning happens early in the BeanFactory initialization

---

### Spring Expression Language (SpEL)

<p align="left">
PropertyAccessor implementations that specify target types for which they should apply now properly take precedence over generic, fallback property accessors such as the ReflectivePropertyAccessor. Consequently, the order in which accessors are evaluated may change when upgrading to Spring Framework 6.2. If you notice unexpected behavior for property access in SpEL expressions, you may need to revise the canRead() and canWrite() implementations of the property accessors used in your application or register accessors in a different order.
</p>

---

### Web Applications

<p align="left">
Static resource locations now have a trailing slash appended if not present.
</p>
<p align="left">
org.webjars:webjars-locator-core support implemented in WebJarsResourceResolver is deprecated due to efficiency issues as of Spring Framework 6.2 and is superseded by org.webjars:webjars-locator-lite support implemented in LiteWebJarsResourceResolver.
</p>

---

### Support for Fallback beans

```java
@Configuration
class MyConfiguration {

	@Bean
	MyComponent myComponent(MyService service) {
    	//...
	}

	@Bean
	@Fallback
	MyService defaultMyService() {
    	//...
	}
}
```
> A fallback bean is used if no bean of that type has been provided.

---

### Background bean initialization

```java
@Configuration
class MyConfiguration {

    @Bean(bootstrap = BACKGROUND)
    MyExpensiveComponent myComponent() {
   	 ...
    }

}
```
> Individual beans can be initialized in the background using the newly introduced bootstrap attribute.

---

### Fragment Rendering

<p align="left">
Spring MVC and WebFlux support rendering multiple views in one request, or to create a stream of rendered views. This helps to support HTML-over-the-wire libraries such as htmx.org and @hotwired/turbo.</p>