# Cấu hình Unicode trong ứng dụng Spring MVC

# 1. Cơ sở dữ liệu (MySQL)

Tạo schema (database) mới có hỗ trợ Unicode

Qua giao diện MySQL WorkBench

![C%20u%20h%20nh%20Unicode%20trong%20ng%20d%20ng%20Spring%20MVC%2013cb0a3a99474b03a3620b117de1b430/Screen_Shot_2020-04-11_at_10.38.18_AM.png](C%20u%20h%20nh%20Unicode%20trong%20ng%20d%20ng%20Spring%20MVC%2013cb0a3a99474b03a3620b117de1b430/Screen_Shot_2020-04-11_at_10.38.18_AM.png)

Hoặc tạo schema qua dòng lệnh

```sql
CREATE SCHEMA `blog` DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci ;
```

# 2. Cấu hình

## 2.1. Kết nối CSDLff

Hỗ trợ Unicode trong connection string thiết lập kết nối đến cơ sở dữ liệu

```java
@Bean
    public DataSource dataSource() {
				...
        dataSource.setUrl("jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf-8");
				...
        return dataSource;
    }
```

## 2.2. Cấu hình Application Server

### Trường hợp 1 - Cấu hình qua file `web.xml`

```xml
<filter>  
    <filter-name>encodingFilter</filter-name>  
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
    <init-param>  
       <param-name>encoding</param-name>  
       <param-value>UTF-8</param-value>  
    </init-param>  
    <init-param>  
       <param-name>forceEncoding</param-name>  
       <param-value>true</param-value>  
    </init-param>  
</filter>  
<filter-mapping>  
    <filter-name>encodingFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```

### Trường hợp 2 - Cấu hình qua class

```java
public class AppInit extends AbstractAnnotationConfigDispatcherServletInitializer {

*// Cách 1*
		@Override
    protected Filter[] getServletFilters() {
        Filter[] filters;
        CharacterEncodingFilter encFilter = new CharacterEncodingFilter();
        encFilter.setEncoding("UTF-8");
        encFilter.setForceEncoding(true);

        filters = new Filter[] {encFilter};
        return filters;
    }

// ===========================================================================

// Cách 2
	@Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        FilterRegistration.Dynamic filterRegistration =
                servletContext.addFilter("endcoding-filter", new CharacterEncodingFilter());
        filterRegistration.setInitParameter("encoding", "UTF-8");
        filterRegistration.setInitParameter("forceEncoding", "true");

        // make sure encodingFilter is matched most first, by "false" arg
        filterRegistration.addMappingForUrlPatterns(null, false, "/*");

        super.onStartup(servletContext);
    }
}
```

### Lưu ý khi sử dụng Spring Security trong dự án

Nếu có sử dụng Spring Security trong dự án và có sử dụng csrf() thì cần bổ sung phần cấu hình sau vào `configure`: 

```java
@Configuration
@EnableWebMvcSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        filter.setForceEncoding(true);
        http.addFilterBefore(filter,CsrfFilter.class);
        // tiếp tục viết code ở đây
    }

		// tiếp tục viết code ở đây

}
```

Hướng dẫn: 

[CharacterEncodingFilter don't work together with Spring Security 3.2.0](https://stackoverflow.com/questions/20863489/characterencodingfilter-dont-work-together-with-spring-security-3-2-0)

## 2.3. Cấu hình View

```java
		@Bean
    public ThymeleafViewResolver viewResolver() {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
				...
        viewResolver.setCharacterEncoding("UTF-8");
				...
        return viewResolver;
    }
```