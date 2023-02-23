# Regular expressions

Regular expressions are a powerful tool for text processing. They are used in many programming languages, and are also available in the shell. Regular expressions are a domain-specific language (DSL) for describing patterns in text. They are a powerful tool for text processing, and are used in many programming languages, and are also available in the shell.

## Syntax

- [A-z] - any letter
- [0-9] - any digit
- [A-z0-9] - any letter or digit
- [^A-z] - any character that is not a letter
- [^0-9] - any character that is not a digit   
- [^A-z0-9] - any character that is not a letter or digit
- [A-z0-9_] - any letter, digit or underscore
...

## Examples

Find a word that starts with a capital letter:
    grep -E "[A-Z][a-z]+" file.txt

Find a word that starts with a capital letter and ends with a dot:
    grep -E "[A-Z][a-z]+." file.txt


## Links

- [Regular expressions](https://en.wikipedia.org/wiki/Regular_expression)
- [Regular expressions 101](https://regex101.com/)