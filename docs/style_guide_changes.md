## Outline

1. [Summary](#summary)
2. [Changes](#changes)
   1. [Comments](#comments)
   2. [Indentation](#indentation)
   3. [Braces](#braces)

## Summary

This file outlines the changes made to the [`google_checks.xml`](data/google_checks.xml) file, which was originally obtained from a [file](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml) on Checkstyle's GitHub repository.

Changes were made in order to:

- Reflect nuances in Eastern University's standard Java style guide with respect to [Google's](https://google.github.io/styleguide/javaguide.html) style guide.

- Improve functionality with code quality checks on [CodeGrade](https://www.codegrade.com/).

- Provide corrections tailored at students first learning Java.

- Add better messages to certain warnings explaining what they mean.

## Changes

### Global

The global `Checker` module was set to the `info` severity level. Therefore, unless otherwise specified, all errors will not result in a loss of points in CodeGrade.

```xml
<module name="Checker">
  <property name="severity" value="info"/>
  ...
</module>
```

### Comments

Many of the comments in the original `xml` were removed or modified as necessary. Additionally, many `xml` `module` attributes were rearranged to better suit documentation purposes.

### Indentation

Default indentation is now 4 spaces, rather than 2. This is done in conjunction with the `--aosp` option built-in to Google's [formatter](https://github.com/google/google-java-format).

`xml` changed:

```xml
<module name="Indentation">
  <property name="basicOffset" value="4"/>
  <property name="braceAdjustment" value="4"/>
  <property name="caseIndent" value="4"/>
  <property name="throwsIndent" value="8"/>
  <property name="lineWrappingIndentation" value="8"/>
  <property name="arrayInitIndent" value="4"/>
</module>
```

_All values are doubled from the original._

### Braces

Brackets are not required on single-line if statements.

**Note:** The only times brackets should be omitted are when:

1. A single `if` is used (no `else`) and the statement can be placed on the same line as the `if`.

Good:

```java
if (condition) statement;
```

Bad:

```java
if (condition) statement;
else other_statement;
```

```java
if (condition) very_long_statement_that_exceeds_line_length
```

2. An `if-else` block (or a series of `else if` conditions) are used together, and **all** of them contain only one statement.

Good:

```java
if (condition1)
    statement1;
else if (condition2)
    statement2;
else if (condition3)
    statement3;
else
    final_statement;
```

Bad:

```java
if (condition)
    statement;
else {
    statement1;
    statement2;
}
```

In each of these situations, omitting brackets is not required. But if it is possible to omit brackets according to these requirements, then doing so is encouraged.

The `xml` was changed to enable the `allowSingleLineStatement` property and set the severity level to `warning`. The affected tokens were also expanded to include every available token.

```xml
<module name="NeedBraces">
  <property name="tokens"
    value="LITERAL_DO, LITERAL_ELSE, LITERAL_FOR, LITERAL_IF, LITERAL_WHILE,
         LITERAL_CASE, LITERAL_DEFAULT, LAMBDA"/>
  <property name="allowSingleLineStatement" value="true"/>
  <property name="severity" value="warning"/>
</module>
```

### Naming

Naming conventions are predominately based on Google's style guide, with some exceptions. For example, underscores are now strictly prohibited (except in constant names).

In _most_ cases, either [camel](https://en.wikipedia.org/wiki/Camel_case) or Pascal case are strictly enforced.

#### Camel Case Modules

A number of modules were changed in the following ways. They now:

1. Require camel case (without any underscores) using a consistent regular expression.
2. Have nearly identical messages that are designed to be clear for students.
3. Are set to the `error` severity level.
4. Are located in the same section of the `xml` file, under the "camel case" header.

These modules include the following:

- `CatchParameterName`
- `LambdaParameterName`
- `LocalVariableName`
- `MemberName`
- `MethodName`
- `ParameterName`
- `PatternVariableName`
- `RecordComponentName`

#### Pascal Case Modules

The `TypeName` module was changed in a similar fashion, except that it requires Pascal case.

#### All Capitals

These modules, which effect the naming of generics, require only capital letters and numbers. This is significantly more restrictive than Google's [requirements](https://google.github.io/styleguide/javaguide.html#s5.2.8-type-variable-names). They include:

- `ClassTypeParameterName`
- `RecordTypeParameterName`
- `MethodTypeParameterName`
- `InterfaceTypeParameterName`

These modules all use the severity level `error`.

#### Constants

The `ConstantName` module was added and set to require upper snake case. However, since constant naming [cannot](https://checkstyle.org/google_style.html#a5.2.4) be enforced perfectly in accord with Google's [style](https://google.github.io/styleguide/javaguide.html#s5.2.4-constant-names), the severity level is set to `info`, which does not cause students to lose points in CodeGrade.

#### Packages

The `PackageName` module was kept using Google's [style](https://google.github.io/styleguide/javaguide.html#s5.2.1-package-names) of only lowercase letters and numbers. Its severity level was set to `error`.

#### Other Naming Changes

The `AbbreviationAsWordInName` module was changed to disable ignoring `static` names:

```xml
<property name="ignoreStatic" value="false"/>
```

It was set to the severity level `error`.

### Javadocs

The `MissingJavadocMethod` module was modified to exclude the `main` method from requiring Javadocs. It was also set to the `error` severity level.

```xml
<property name="ignoreMethodNamesRegex" value="^main$"/>
<property name="severity" value="error"/>
```