# json-fixer
Accepts file paths as arguments. Each file is then checked to make sure it
exists, is a file, and contains valid JSON; if it does, it is prettified in
situ. The name *fixer* is misleading as this tool does not *fix* anything, it
simply makes JSON files pretty. The name is derived from [php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer), which applies coding standards to PHP
files.

**Signiture:**

`json-fixer [input-file] ...`

## Installation

Download the file…

```
$ curl -L https://raw.githubusercontent.com/garettrobson/json-fixer/master/json-fixer -o json-fixer
```

… make it executable…

```
$ sudo chmod a+x json-fixer
```

… and put it in an accessible path.

```
$ sudo mv json-fixer /usr/local/bin/json-fixer
```

## Basic example

Imagine you have a messy/packed json file that you would like to make readable.

```
{"foo":{"bar":"baz"},"qux":["quux","quuz","corge"],"grault":"https://github.com/garettrobson/json-fixer","garply":1024,"waldo":true,"fred":false,"plugh":null,"xyzzy":3.14159265359,"thud":"\\\\samba-share\\some\\directory"}
```

Simply run json-fixer passing it the files path as a parameter.

```
$ json-fixer example.json
Fixed example.json
```

```
$ cat example.json
{
    "foo": {
        "bar": "baz"
    },
    "qux": [
        "quux",
        "quuz",
        "corge"
    ],
    "grault": "https://github.com/garettrobson/json-fixer",
    "garply": 1024,
    "waldo": true,
    "fred": false,
    "plugh": null,
    "xyzzy": 3.14159265359,
    "thud": "\\\\samba-share\\some\\directory"
}
```


## Usage

You can run the fixer on multiple files given specific paths.

```
$ json-fixer \
    relative/path/file.json \
    /absolute/path/file.json \
    *.json
```

This tool **will not** recurse directories. To perform such a task it is best to
use `find` and pipe it to `xargs json-fixer`;

```
find . -name "*.json" | xargs json-fixer
```

If you try and fix a file which does not contain json, or is invalid json, you
will receive a minimal error with a brief explanation.

```
$ json-fixer \
    /root \
    invalid.json \
    spaghetti.jpg
```

```
Specified path /root is not a file
Failed to decode invalid.json due to Syntax error (4)
Failed to decode spaghetti.jpg due to Malformed UTF-8 characters, possibly incorrectly encoded (5)
```

Please note that the number in brackets is a json decoding error code, and does
not relate to a line number or any other information which would be useful in
debugging the issue. Please use the [jq](https://stedolan.github.io/jq/)
command, or similar command, for ascertaining such debugging information.
