# 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술

아래 내용은 인프런 **스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술 (김영한)** 강의를 듣고 정리한 내용입니다.

## 1. 서블릿

> 자바를 사용하여 웹을 만들기 위해 필요한 기술입니다. 클라이언트가 어떠한 요청을 하면 그에 대한 결과를 다시 전송해주는 역할을 하는 자바 프로그램입니다.

```java
@ServletComponentScan // 서블릿 구성요소에 대한 scan을 활성화 하며, 내장 웹 서버를 사용하는 경우에만 스캔을 수행한
@SpringBootApplication
public class ServletApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServletApplication.class, args);
	}
}
```

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {

        System.out.println("Hello Servlet Service");
        System.out.println("request = " + request );
        System.out.println("response = " + response );

        String username = request.getParameter("username");
        System.out.println("username = "+ username );

        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        response.getWriter().write("Hello "+ username);
    }
}
```

HttpServletRequest / HttpServletResponse : HTTP 요청 메세지를 개발자가 직접 파싱해서 사용해도 되지만 메세지를 편리하게 사용할 수 있도록 HttpServletRequest 객체에 담아서 사용한다. 또한 부가기능 임시 저장소 기능이 있다. request.setAttrivute(name, value) / request.getAttribute(name) ~~알고보면 교육에서 배우고 사용했던 기능인데 정확히 어떤 의미로 사용되는 지 몰랐다..~~



## 2. HTTP 요청 데이터

**GET - 쿼리 파라미터** : 메세지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달한다. 검색, 필터, 페이징등에서 많이 사용하는 방식이다. 클라이언트에서 서버로 데이터를 전달할 때 HTTP 바디를 사용하지 않기 때문에 content-type이 없다.

```java
/*
http://localhost:8080/request-param?username=hello&username=kim&age=20
*/
@WebServlet(name = "requestParamServlet", urlPatterns = "/request-param")
public class RequestParamServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {

        /* 전체 파라미터 조회 */
        request.getParameterNames().asIterator()
            .forEachRemaining(paramName -> System.out.println(paramName+ "=" + request.getParameter(paramName)));

        /* 단일 파라미터 조회 */
        String username = request.getParameter("username");

        /* 중복 된 이름으로 들어온 파라미터 값 */
        String[] usernames = request.getParameterValues("username");
        for (String name : usernames){
            System.out.println("name = " + name);
        }
    }
}
```

**POST - HTML Form** : 메세지 바디에 쿼리 파라미터 형식으로 전달한다. HTML Form 사용하기 때문에 바디에 포함된 데이터가 어떤 형식인지 content-tpye : application/x-www-form-urlencoded 을 꼭 지정해야 한다. 

**HTTP message body** : HTTP API에서 주로 사용한다. JSON, XML, TEXT 데이터 형식은 주로 JSON을 사용한다.

```java
@WebServlet(name = "requestJsonServlet", urlPatterns = "/request-json")
public class RequestJsonServlet extends HttpServlet {

    ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {

        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
        System.out.println("helloData.getUsername() = " + helloData.getUsername());
        System.out.println("helloData.getAge() = " + helloData.getAge());
    }
}
```



### 3. HTTP 응답 데이터

HTTP 응답코드 지정 / 헤더 생성 / 바디 생성 / Content-Type, 쿠키, Redirect 편의 기능 제공


