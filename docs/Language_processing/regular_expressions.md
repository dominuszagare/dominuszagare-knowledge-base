# Regular expressions

Regular expressions are a powerful tool for text processing. They are used in many programming languages, and are also available in the shell. Regular expressions are a domain-specific language (DSL) for describing patterns in text.

## Syntax for extended regular expressions

- `.` - match any character
- `^` - match the beginning of a line
- `$` - match the end of a line
- `*` - match zero or more occurrences of the preceding expression
- `+` - match one or more occurrences of the preceding expression
- `?` - match zero or one occurrences of the preceding expression
- `|` - match either of the expressions
- `()` - group expressions (usually to define subpatterns)
- `{}` - match a specific number of occurrences of the preceding expression
- `[]` - match any character in the brackets
- `[^]` - match any character not in the brackets
- `\` - escape special characters
- `[A-Za-z]` - any letter
- `[0-9]` - any digit
- `[A-Za-z0-9]` - any letter or digit
- `[^A-Za-z]` - any character that is not a letter
- `[^0-9]` - any character that is not a digit   
- `Abc+` - one or more occurrences of `Abc`

## ECMAScript syntax

[ECMAScript](https://cplusplus.com/reference/regex/ECMAScript/) is a regular expression syntax used in JavaScript and other languages. It is similar to the POSIX syntax, but with some differences.

### special characters

- `\t` Tab
- `\n` Newline
- `\r` Carriage return
- `\v` Vertical tab
- `\f` Form feed
- `\0` Null character
- `\xhh` Character with hexadecimal value hh (00–FF)
- `\uhhhh` Character with hexadecimal value hhhh (0000–FFFF)
- `\d` digit
- `\D` not digit
- `\s` whitespace
- `\S` not whitespace
- `\w` word
- `\W` not word
- `\b` word boundary
- `\B` not word boundary
- `[class]` charachter clas 
- `[^class]` negated character class

### character classes

[:alnum:] - alphanumeric characters
[:alpha:] - alphabetic characters
[:blank:] - space and tab characters
[:cntrl:] - control characters
[:digit:] - digit characters
[:graph:] - printable characters excluding space
[:lower:] - lowercase letters
[:print:] - printable characters including space
[:punct:] - punctuation characters
[:space:] - whitespace characters
[:upper:] - uppercase letters
[:xdigit:] - hexadecimal digit characters
## Links

- [Regex tester](https://www.regextester.com/96872)
- [Regular expressions](https://en.wikipedia.org/wiki/Regular_expression)
- [Regular expressions 101](https://regex101.com/)
- [Regular expressions in Python](https://docs.python.org/3/library/re.html)
- [Regular expressions in Bash](https://tldp.org/LDP/abs/html/x17129.html)

## Bash analyzing large amounts of files

- [Bash iterating through files](https://www.digitalocean.com/community/tutorials/workflow-loop-through-files-in-a-directory)
- [Regular expressions in grep](https://www.cyberciti.biz/faq/grep-regular-expressions/)

### grep flags

- `-E` - use extended regular expressions
- `-i` - ignore case
- `-n` - print line numbers
- `-o` - print only the matched parts of a matching line, with each such part on a separate output line
- `-v` - select non-matching lines
- `-w` - match the whole word
- `-x` - match the whole line
- `-c` - print only a count of matching lines for each input file
- `-l` - print only names of files containing matches, not the matching lines
- `-L` - print only names of files not containing matches
- `-m` - stop reading a file after NUM matching lines
- `-A` - print NUM lines of trailing context after matching lines
- `-B` - print NUM lines of leading context before matching lines
- `-C` - print NUM lines of output context
- `-e` - add PATTERN to the list of patterns to search for
- `-f` - obtain patterns from FILE, one per line
- `-r` - read all files under each directory, recursively
- `-R` - same as -r

1. Find all files in a directory that end with `.txt` or `.text.xml` and print them to the screen:
    ```bash
    for FILE in *\.text\.xml *\.txt; do echo $FILE; done
    ```
    or
    ```bash
    find . -type f -name "*.txt" -o -name "*.text.xml"
    ```

2. Search for relevant patterns in all files in a directory:
    ```bash
    for FILE in *\.text\.xml *\.txt; do grep -E "[A-Z][a-z]+" $FILE; done
    ```
    or
    ```bash
    # find all words that start with a capital letter
    grep -E -o "[A-Z][a-z]+" *\.text\.xml *\.txt
    ```

#### grep examples

Find email addresses:
    grep -E -o '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}' *\.text\.xml

Find all words that start with a capital letter and end with a dot:
    grep -E '[A-Z][a-z]*\.' *\.text\.xml

## Rust analyzing large amounts of files

- [Rust iterating through files](https://doc.rust-lang.org/std/fs/fn.read_dir.html)
- [Regular expressions in Rust](https://docs.rs/regex/1.7.1/regex/struct.Regex.html)

Syntax is similar to [C++ ECMAScript syntax](https://cplusplus.com/reference/regex/ECMAScript/)

This implementation of regex doesn't support `<=`look ahead or `=>` behind expression. 
`r` and `t` are Rust lifetimes of a compiled regular expression and text to search, respectively.
All searching is done with an implicit `.*?` at the beginning and end of an expression. 

To force an expression to match the whole string (or a prefix or a suffix), you must use an anchor like `^` or `$` (or `\A` and `\z`).

### Rust examples

Find all words that start with a capital letter and end with a dot:
```rust
    use regex::Regex;
    use std::fs::File;
    use std::io::{BufRead, BufReader};

    let re = Regex::new(r"[A-Z][a-z]*\.").unwrap();
    let file = File::open("file.txt").unwrap();
    let reader = BufReader::new(file);

    for line in reader.lines() {
        let line = line.unwrap();
        if re.is_match(&line) {
            println!("{}", line);
        }
    }
```

### Find XML tag

```rust
    let s = "Hello, world!";
    let hello = &s[0..5];
    let world = &s[7..12];
```

### Get substring position

```rust
    fn get_xml_tags(data: &String, tag: &str) -> Vec<String> {
    let mut matches = Vec::new();
    if(data.is_empty()){
        return matches;
    }
    let re = Regex::new(&format!(r"(<\s*{}\s*.+?(/>|(>(.|\n)*?</\s*{}\s*>)))",tag,tag));
    if(re.is_err()){
        println!("Error in regex: {:?}",re.err());
        return matches;
    }
    //get all matches
    for cap in re.unwrap().captures_iter(data) {
        if cap.len() > 0 {
            matches.push(cap[0].to_string());
        }
    }
    matches
}
```

