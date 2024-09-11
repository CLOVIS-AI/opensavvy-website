# OpenSavvy Code Style

This page contains a description of the OpenSavvy language-agnostic coding style.
Generally, the coding style for each language is a mix of our general coding style and the official coding style for the language.

The goal is for adoption to be easy, while still providing a set of common rules, to ensure familiarity between the codebases, even if they are in different languages.

!!! tip "For IntelliJ-based IDEs"
    For IntelliJ-based IDEs (Android Studio, PyCharm, PHPStorm…) you can get the up-to-date configuration [here](https://gitlab.com/opensavvy/playgrounds/baseline/-/tree/main/.idea?ref_type=heads). You can either copy/paste this directory into your project, or use "Settings → Editor → Code Style" then "Import Scheme" to import the configuration into your IDE.

The rest of this document describes rules that are common to all languages.

## Text formatting

### Trailing spaces

Lines should not end with trailing whitespace.

!!! tip "For IntelliJ-based IDEs"
    Open "Settings → Editor → General", "On save", tick "Remove line breaks on modified lines".

### End of files

All files should end with a proper line terminating character.
Improperly terminated files make diffs harder to read, and cause display problems in terminals.

Generated files which are not meant to be edited by humans are exempt from this rule.

!!! tip "For IntelliJ-based IDEs"
    Open "Settings → Editor → General", "On save", tick "Remove trailing blank lines at the end of saved files" and "Ensure every saved file ends with a line break".

### Indentation

We believe [indent size is like light or dark theme](https://ivan.canet.dev/blog/2024/07/03/tabs-v-spaces.html): everyone has their personal preference of what they find more readable, _and it should not impact other contributors_.

To satisfy all sizes, we use tabulations. Each developer is thus free to select their own display size, without impacting anyone else. Code should not depend on the indent size, so vertical alignment is discouraged.

!!! tip
    Modern tools allow you to configure how large you want tabulations to be displayed.

    - GitLab: "tab width" in [settings](https://gitlab.com/-/profile/preferences#behavior).
    - GitHub: "tab size preference" in [settings](https://github.com/settings/appearance).
    - For IntelliJ-based IDEs, this is configured as part of each language's coding style.

If a line is continued in a following line, it should be indented at one more level than the header line.

```javascript
const a = "first line " +
	"second line " +
	"third line";
```

## Spacing

### Whitespace

Put a space between keywords and their parentheses, but not between functions and theirs.
```c
if (something) {
	execute();
}
```

Do not put a space between delimiters of an argument list.
Put a space after list delimiters, but not before.
```c
execute(5, 1, 2); other(1, 2);
```

Put spaces around binary operators.
Do not put a space between a unary operator and its target, but do put one on the other side.
```c
int a = 2 * 3 + i++ + ++j;
```

### Braces and blocks

When marking the start of a section and the end of a section with characters, the starting character should be at the end of the opening line, each element should be indented on its own line, and the ending character should be unindented on its own line.

```c
if (a == 2) {
	printf("A: %d", a);
}
```

When the closing line has its own keyword, it should be added on the same as the ending character.

```c
if (something) {
	foo();
} else {
	bar();
}
```

```c
do {
	foo()
} while (something);
```

### Long lines

Lines of at most 80 characters are preferred. Lines should not be longer than 120 characters, except long strings (to ensure they can be grepped easily).

When breaking up parameter lists (e.g. a function declarations), make each element in the list its own line:

```kotlin
// OK: it's less than 80 characters
fun foo(a: Int, b: Int, c: Int) = a + b + c

// OK: each element is on its own line
fun foo(
	a: Int,
	b: Int,
	c: Int,
) = a + b + c

// BAD: mix of both
fun foo(
	a: Int, b: Int,
	c: Int,
) = a + b + c
```

When broken on multiple lines and supported by the language, the last element should end with the same separator as used to between the previous elements. For example, in the above examples, elements are separated by commas, so the last element's line should also end with a comma. This helps reduce the size of file diffs and reduce conflicts when multiple patches add elements to the list.

The trailing separator is not used when all elements are on a single line.
