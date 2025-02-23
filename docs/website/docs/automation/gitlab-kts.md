# Kotlin for GitLab CI (Beta)

Generate your GitLab CI pipeline in Kotlin instead of Yaml!

Built-in plugins allow creating complex pipelines from simple configuration, with preconfigured caching, test reporting, and authentication. 
```kotlin
gitlabCi {
    val test by stage()
    
    val build by job(stage = test) {
        useGradle()

        script {
            gradlew.task("build")
        }
    }
}.println()
```

```yaml
stages:
  - test

build: 
  stage: test
  variables: 
    GRADLE_USER_HOME: $CI_BUILDS_DIR/.gradle
  cache: 
    paths: 
      - .gradle/wrapper
    key: 
      files: 
        - gradle/wrapper/gradle-wrapper.properties
  script: 
    - ./gradlew build
  artifacts: 
    reports: 
      junit: '**/build/test-results/**/TEST-*.xml'
    when: always
```

<div class="grid cards" markdown>

- [Website](https://opensavvy.gitlab.io/automation/gitlab-ci.kt/docs/)
- [Repository](https://gitlab.com/opensavvy/automation/gitlab-ci.kt)
- Licensed under **Apache 2.0**

</div>
