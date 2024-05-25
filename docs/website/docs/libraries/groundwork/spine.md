# Spine (Alpha)

[Ktor](https://ktor.io/) has greatly simplified multiplatform HTTP development by providing symmetric DSLs and allowing sharing DTOs between clients and servers.

Spine adds a small DSL to declare endpoints and their signature in code shared between client and server, which provides many benefits:

- Two-click two-way navigation between client and server code.
- Never accidentally use different DTOs on client and server code.
- Never accidentally use different query parameters on client and server code.
- Fearlessly refactor your API without forgetting to change something on the other side.
- Use KDoc to document your endpoints, and see the documentation directly in your IDE.
- …and many other IDE features that become automatically available simply because we declare everything as code!

```kotlin
object Users : StaticResource("users", parent = ApiRoot) {
	class ListParameters(data: ParameterStorage) : Parameters(data) {
		var includeInactive by parameter(default = false)
	}
	
	val list by get()
		.parameters(::ListParameters)
		.response<User>()
	
	val create by post()
		.request<User>()
		.response<User>()
}

// Server-side
routing {
	route(Users.create) {
		respond(userService.create(user = body), HttpStatusCode.Created)
	}
}

// Client-side
httpClient.request(Users.create).isSuccessful()
```

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/groundwork/spine)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)

</div>
