# kotlin-grammar-tools

[![TeamCity (simple build status)](https://img.shields.io/teamcity/https/teamcity.jetbrains.com/e/Kotlin_Spec_GrammarMaster.svg?style=flat)](https://teamcity.jetbrains.com/viewType.html?buildTypeId=Kotlin_Spec_GrammarMaster&branch_Kotlin_dev=%3Cdefault%3E&tab=buildTypeStatusDiv)

## Description

This library allows tokenize and parse Kotlin code in your program using the Kotlin grammar.

Simple example:
```kotlin
fun main() {
    val tokens = tokenizeKotlinCode("val x = foo() + 10;")
    val parseTree = parseKotlinCode(tokens)
    // or just `val parseTree = parseKotlinCode("val x = foo() + 10;")`

    println(parseTree)
}
```

Tokens or parse tree can be used for various Kotlin code analysis.

Note that the parse tree may not match exactly to PSI (parse tree generated by Kotlin compiler).
This is due to the fact that some errors for the user convenience are not generated at the parser level, but later; the grammar, in turn, takes into account such cases and may not allow the code that could be parsed by the Kotlin compiler parser.

### Kotlin grammar

The grammar is located in the [Kotlin specification repository](https://github.com/JetBrains/kotlin-spec/tree/master/grammar).

## Status

The library is developed **only for internal purposes** of the Kotlin team, and actual state of the library isn't guaranteed.

## Using

To use the library you can run the `publishToMavenLocal` gradle task and add `mavenLocal` to repositories in your program, and then add dependency from this library. For example (gradle): `implementation("org.jetbrains.kotlin.spec.grammar.tools:kotlin-grammar-tools:0.1")`.

Also, you can just download jar from [Releases](https://github.com/Kotlin/kotlin-grammar-tools/releases) or TeamCity (`kotlin-grammar-tools` artifacts can be found on the [TeamCity Kotlin grammar builds pages](https://teamcity.jetbrains.com/viewType.html?buildTypeId=Kotlin_Spec_GrammarMaster)).

## Exceptions

Lexer and parser are throwing exceptions if it has been inputted the invalid code (in terms of lexer or parser): `KotlinLexerException` and `KotlinParserException`.

Example of handling this exceptions:
```kotlin
fun foo(): ParseTree? {
    val tokens = try {
        tokenizeKotlinCode("val x = foo() + 10;")
    } catch (e: KotlinLexerException) {
        println("Tokenization the code fails")
        return null
    }
    val parseTree = try {
        parseKotlinCode(tokens)
    } catch (e: KotlinParserException) {
        println("Parsing the code fails")
        return null
    }

    return parseTree
}
```
