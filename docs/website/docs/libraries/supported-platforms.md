# Supported platforms

Kotlin Multiplatform supports a [wide range of platforms](https://www.jetbrains.com/help/kotlin-multiplatform-dev/supported-platforms.html).

We are, first and foremost, developers working in our free time, so we cannot support all of those.
Here are the platforms we currently strive to support:

- `jvm` ([minimum supported Java version](https://gitlab.com/opensavvy/automation/gradle-conventions/-/blob/main/versions/src/main/kotlin/Versions.kt))
- `js` (IR only)
- `linuxX64`
- `iosX64`
- `iosArm64`
- `iosSimulatorArm64`

!!! info
    In the rest of this website, the mention "Supports most Kotlin platforms" refers to supporting the subset above. 

## Adding support for other platforms

If there is another platform we would like us to support, [you can request it here](https://gitlab.com/opensavvy/playgrounds/gradle/-/issues/new) (or [search for existing requests](https://gitlab.com/opensavvy/playgrounds/gradle/-/issues/?sort=priority&state=all&label_name%5B%5D=platforms&first_page_size=20)).

We strive to avoid platform-specific code as much as possible, so most of our projects are easy to port to new platforms. The main difficulty is often figuring how to automatically produce all artifacts; if you can help with this, your request is much more likely to proceed quickly.
