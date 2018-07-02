# Parenthesis and other nested structures

## Get everything between parenthesis

```
\[(.*?)\]
```

## Get everything between C/C++ multiline comments /* */

```
\/\*[\S\s]*?\*\/
```
## Get everything between single quotes allowing inline escaping using the rolling loop technique

```
'((?:[^\\']|\\.)*)'
```

## Check that string CONTAINS a valid hierarchy of parenthesis or not

```
\(((?>[^()]+)|(?R))*\)
```

## Get everything between braces not enclosed by quotes
```
("|').*?\1(*SKIP)(*FAIL)|\{(?:[^{}]|(?R))*\}
```
... allowing escaping special characters
```
("|').*?\1(*SKIP)(*FAIL)|{(?>[^{}\\]++|\\.|(?R))*+}
```

## Get everything between parenthesis allowing escaping special characters
```
(\((?>[^()\\]++|\\.|(?1))*+\))
```

## Matching 3 different types of balanced parentheses with .NET balancing groups

```
(
    [^(){}\[\]]+
    | \( (?=[^)]*  (?<Stack> \) ) )
    | \[ (?=[^\]]* (?<Stack> \] ) )
    | \{ (?=[^}]*  (?<Stack> \} ) )
    | \k<Stack> (?<-Stack>)
)+?
(?(Stack) (?!))
```

## Get everything between double quotes

```
"[^"]+"
```

### .. quotes allowing escaped quotes

#### PCRE (quickest)
```
["'](?:(?<=")[^"\\]*(?s:\\.[^"\\]*)*"|(?<=')[^'\\]*(?s:\\.[^'\\]*)*')
```
`(?s:...)` is a syntactic sugar to switch on the dotall/singleline mode inside the non-capturing group. If this syntax is not supported use a flag to switch this mode on for all the pattern or replace the dot with `[\s\S])`

#### ECMA script:
```
(?=["'])(?:"[^"\\]*(?:\\[\s\S][^"\\]*)*"|'[^'\\]*(?:\\[\s\S][^'\\]*)*')
```
#### POSIX extended:
```
"[^"\\]*(\\(.|\n)[^"\\]*)*"|'[^'\\]*(\\(.|\n)[^'\\]*)*'
```
or simply:
```
"([^"\\]|\\.|\\\n)*"|'([^'\\]|\\.|\\\n)*'
```

#### using regex recurision (slow)
```
(["'])(?:(?=(\\?))\2.)*?\1
```
`([""'])` match a quote; `((?=(\\?))\2.)` if backslash exists, gobble it, and whether or not that happens, match a character; `*?` match many times (non-greedily, as to not eat the closing quote); `\1` match the same quote that was use for opening.  

Optimized version
```
(['"])(?:(?!\1|\\).|\\.)*\1
```