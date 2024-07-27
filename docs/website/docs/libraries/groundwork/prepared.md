# Prepared

Prepared is a multiplatform DSL for declaring automated tests without using code generation or annotation magic, while adding many advanced features such as isolated test fixtures, virtual time control, randomness control, etc.

```kotlin
val database by prepared {
	Database.connectLocal()
		.also { cleanUp { it.close() } }
}

suite("Test users") {
	val guest by prepared {
		database().createGuestUser()
	}

	test("An administrator should be able to view guests") {
		database().listUsersWithRight(Role.Administrator) shouldContain guest()
	}

	test("A guest shouldn't be able to view other guests") {
		database().listUsersWithRight(Role.Guest) shouldNotContain guest()
	}
}
```

<div class="grid cards" markdown>

- [Website](https://opensavvy.gitlab.io/groundwork/prepared/docs)
- [Repository](https://gitlab.com/opensavvy/groundwork/prepared)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)
- Supports coroutines

</div>
