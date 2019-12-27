# spring-boot-microservices-zuul
Spring Boot Microservices Zuul API Gateway

# Eureka Server
    URL - http://localhost:8761

# Eureka Client - 1
    URL - http://localhost:8001/eureka-client-1

# Eureka Client - 2
    URL - http://localhost:8002/eureka-client-2

# Eureka Client - 3
    URL - http://localhost:8003/eureka-client-3

# Zuul API Gateway
Here are the steps need to follow to enable Zuul API Gateway

1 - pom.xml - zuul dependency

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    </dependency>
2 - application.properties

    zuul.routes.eureka-client-2.url=http://localhost:8002/eureka-client-2
    zuul.routes.eureka-client-3.url=http://localhost:8003/eureka-client-3

3 - Application.java

    1. Add @EnableZuulProxy annotation at class level
    2. Create @Bean 

    @Bean
	public ZuulFilter postFilter() {
		return new ZuulFilter() {

			@Override
			public boolean shouldFilter() {
				return true;
			}

			@Override
			public Object run() throws ZuulException {
				System.out.println("Pre Filter - run");
				HttpServletRequest request = RequestContext.getCurrentContext().getRequest();
				System.out.println("Request Method : "+request.getMethod());
				System.out.println("Request URL : "+request.getRequestURL().toString());
				return null;
			}

			@Override
			public String filterType() {
				return "pre";
			}

			@Override
			public int filterOrder() {
				return 1;
			}
		};
	}