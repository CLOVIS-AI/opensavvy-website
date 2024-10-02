# Stability levels

Stable OpenSavvy projects follow [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html). However, not all projects are stable, and stable projects sometimes contain unstable components. This document outlines the rules between all these combinations.

The stability level is only an indication of our vision of the project. After all, we are doing this in our free time. We may change our minds at any time—since most of our projects are open source, you have the ability to fork the project.

Projects which are not marked specifically in relation to this document may not respect its contents, for example if they were written before this document. If you encounter one, we recommend contacting us to ask for clarification before assuming any specific stability level.

## Project stability

These labels are given to an entire repository, or to a project in a repository.

[![Stability: experimental](https://badgen.net/static/Stability/experimental/purple)](https://opensavvy.dev/open-source/stability.html)
[![Stability: alpha](https://badgen.net/static/Stability/alpha/purple)](https://opensavvy.dev/open-source/stability.html)
[![Stability: beta](https://badgen.net/static/Stability/beta/purple)](https://opensavvy.dev/open-source/stability.html)
[![Stability: stable](https://badgen.net/static/Stability/stable/purple)](https://opensavvy.dev/open-source/stability.html)
[![Stability: deprecated](https://badgen.net/static/Stability/deprecated/purple)](https://opensavvy.dev/open-source/stability.html)
[![Stability: archived](https://badgen.net/static/Stability/archived/purple)](https://opensavvy.dev/open-source/stability.html)

- **Experimental**: Only use in toy projects. We have not yet decided if we will pursue this project or not. It is possible that we drop it at any time, without warning. The API surface will most likely change without warning.
- **Alpha**: Use at your own risk. We consider the project to be advanced enough that we use it ourselves, but we don't think it's of enough quality yet to be used wildly (lacking documentation, changing API surface…).
- **Beta**: If you're feeling adventurous. We consider the core of the project to be usable, but it is still lacking in some ways. This is a stabilization phase: it is unlikely that the API surface will change in major ways, but it is still possible.
- **Stable**: We consider the project ready to use in any context. Major changes will go through a deprecation phase and only happen according to the rules of Semantic Versioning.
- **Deprecated**: We have decided to stop working on the project. The project still follows exactly the same rules as if it was Stable, but we likely won't be making any major changes.
- **Archived**: We have entirely stopped working on the project. It is possible we may still make occasional modifications.

Experimental, alpha and beta levels can only happen for an entire repository pre-`1.0.0`, or be marked as a pre-release (e.g. `1.0.0-alpha`). Therefore, version `2.0.0` must be stable or a later level. The pre-release marker doesn't necessarily match the level (e.g. `1.0.0-rc.2` may be experimental, alpha or beta).

!!! note ""
    Specific modules or parts of a project may remain unstable even after `1.0.0`. See [Component stability](#component-stability) for more information. 

There is no level after archived. Since the most likely reason for a project to be archived is that we do not have enough time or are not willing to continue maintaining it, we will most likely keep it world-readable and not delete it entirely.

## Component stability

Even if a project is marked stable, we may still decide to experiment with specific components.
This is allowed by Semantic Versioning, as it makes no mention of what is and isn't a part of the public API—it only defines rules to version the public API.

Components marked as experimental are not considered to be part of the public API in regard to Semantic Versioning.

### Stable

By default, components are not marked in any particular way. These components follow the same stability level as the project they are a part of: it the project is stable, then they are stable as well, and follow Semantic Versioning.

These components can only go through source- and binary-compatible changes.

### Experimental

Experimental components are still in the draft phase. They may be removed or changed at any time, even as part of a patch release.

Because binary compatibility is not guaranteed, using them in a library is particularly dangerous (it may break your user's builds if they upgrade the initial project but not your library).

Source-compatibility is not guaranteed either, but it is generally less risky to use them in final projects (not libraries) because it is easier to migrate away if needs be.

If the technology used in the project has an experimental marker (e.g. [Opt-in requirements](https://kotlinlang.org/docs/opt-in-requirements.html) in Kotlin), it will be used to mark the component. Otherwise, the stability level will be explained in the documentation.

### Deprecated

Deprecated components are planned for removal. If possible, we will provide a migration path away from them.

**Source compatibility protection**:
Components marked as deprecated must remain source-compatible for at least an entire minor version.

Non-stable versions (e.g. `1.2.0-rc.1`) do not have to follow this rule.

!!! example
    If a component is marked as deprecated in the version `1.1.2`, the first source-incompatible stable version is `1.3.0`, because source-compatibility is guaranteed for the entire `1.2.*` minor version range.

    This ensures that users upgrading from multiple versions ago can catch all deprecation warnings by only testing major versions.

**Binary compatibility protection**:
Components marked as deprecated must remain binary-compatible until the next major version after the end of the source compatibility protection.

Non-stable versions (e.g. `1.2.0-rc.1`) do not have to follow this rule.

!!! example
    For example, if a component is marked as deprecated in the version `1.1.2`, and a `1.2.0` version has existed, the first binary-incompatible stable version is `2.0.0`.

    However, if the version `2.0.0` happens directly after the version `1.1.2`, the source-compatibility rule will not have been validated yet, so the component must be binary-compatible until the next major version, `3.0.0` (then, the component will have remained source-compatible for the `2.0.*` range).

To help users understand when things will be deleted, we recommend documenting the version number at which the source compatibility protection, as well as the version number at which the binary compatibility protection ends.

Both rules are a minimum protection guarantee. A project may keep source, binary, or both types of compatibility for longer than written here.

If the technology used in the project has a deprecation marker  (e.g. [Deprecated](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-deprecated/) in Kotlin), it will be used to mark the component. Otherwise, the deprecation will be explained in the documentation.
