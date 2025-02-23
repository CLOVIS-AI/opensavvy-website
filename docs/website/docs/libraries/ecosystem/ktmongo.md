# KtMongo (Alpha)

MongoDB is a popular database because its concepts map cleanly to object-oriented structures, unlike SQL DBMSs. Kotlin's DSL capabilities allow us to create idiomatic and safe drivers.

The official Java and Kotlin drivers do not take advantage of Kotlin's DSLs. [KMongo](https://litote.org/kmongo/) pioneered DSLs for MongoDB, but isn't maintained anymore. **OpenSavvy KtMongo** is our complete rethink of KMongo, from the ground up, with even more safety and even more operators (for example: support for aggregation operators).

```kotlin
@Serializable
data class User(
	val _id: ObjectId,
	val name: String,
	val birthYear: Int,
	val archived: Boolean?,
)

val liveUsers = users.filter { User::archived ne true }

liveUsers.find {
	User::archived ne true
	User::birthYear lt 2000
}
```

<div class="grid cards" markdown>

- [Website](https://opensavvy.gitlab.io/ktmongo/docs)
- [Repository](https://gitlab.com/opensavvy/ktmongo)
- Licensed under **Apache 2.0**
- Supports the **JVM**
- May support more platforms in the future
- Usable with or without coroutines

</div>
