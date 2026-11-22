---
tags:
  - Spring
  - LEAF
---
- Used to monitor and manage your application
- Check your application health
- Access application metrics

Does this through adding endpoints at `/actuator` (is configurable)
By default only `/actuator/health` is exposed you can expose other actuator endpoints using the following in the application properties file:
`management.endpoints.web.exposure.include=*`
value can be: 
- * = all endpoints
- comma delimited list
# Info endpoint
Is customisable and shows information that you input in your application properties as so:
 ```
 info.app.name=My Super Cool App
 info.app.description=A Crazy and Fun app, Yoohoo!
 info.app.version=1.0.0
 ```
 Return
 ```json
{
	"app": {
		"name": "My Super Cool App",
		"description": "A Crazy and Fun app, Yoohoo!",
		"version": "1.0.0",
	}
}
```
 # Security 
 Can be added through the spring security dependency
 # Other actuator endpoints
 https://docs.spring.io/spring-boot/docs/2.1.13.RELEASE/reference/html/production-ready-endpoints.html
 