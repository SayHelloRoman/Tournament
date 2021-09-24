Ваша задача реализовать простой парсер регулярных выражений. У нас будет парсер, который выводит следующий AST регулярного выражения:

```python
Normal(c)           # ^ A character (string) that is not in "()*|."
Any()               # ^ Any character
ZeroOrMore(regexp)  # ^ Zero or more occurances of the same regexp
Or(regexp, regexp)  # ^ A choice between 2 regexps
Str([regexps])      # ^ A sequence of regexps.
```

Как и в случае с обычными регулярными выражениями, Any обозначается символом '.' символа, ZeroOrMore обозначается последующим символом "*", а Or обозначается символом "|". Скобки (и) позволяют группировать последовательность регулярных выражений в объект Str.
Или не ассоциативно, поэтому «a | (a | a)» и «(a | a) | a» являются допустимыми регулярными выражениями, тогда как «a | a | a» - нет.
Приоритеты операторов сверху вниз следующие: *, последовательность и |. Следовательно, справедливо следующее:
```python
"ab*"     -> Str([Normal ('a'), ZeroOrMore(Normal('b'))])
"(ab)*"   -> ZeroOrMore(Str([Normal('a'), Normal('b')]))
"ab|a"    -> Or(Str([Normal('a'), Normal('b')]), Normal('a'))
"a(b|a)"  -> Str([Normal('a'), Or(Normal('b'), Normal('a'))])
"a|b*"    -> Or(Normal('a'), ZeroOrMore(Normal('b')))
"(a|b)*"  -> ZeroOrMore(Or(Normal('a'), Normal('b')))```

Некоторые примеры:

```python
"a"          -> Normal('a')
"ab"         -> Str([Normal('a'), Normal('b')])
"a.*"        -> Str([Normal('a'), ZeroOrMore(Any())])
"(a.*)|(bb)" -> Or(Str([Normal('a'), ZeroOrMore(Any())]), Str([Normal('b'), Normal('b')]))```


Следующее является недопустимым регулярным выражением, и парсер должен возвращать Nothing в Haskell / 0 в C или C ++ / null в JavaScript или C # / None в Python / new Void () в Java / Void () в Kotlin:
