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


