# json-fixer
Accepts file paths as arguments. Each file is then checked to make sure it
exists, is a file, and contains valid JSON; Only if these conditions are met is
it prettified, in situ. Because the files are handled in situ they should
return all ownerships and permissions, only modifying the content and
timestamps.

**Signiture:**

`json-fixer [input-file] ...`

### The name is misleading

The name *fixer* is misleading as this tool does not *fix* anything, it simply
makes JSON files pretty. The name is derived from
[php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer),
which applies coding standards to PHP files. If you want a to actually fix JSON
files tools such as
[jq](https://stedolan.github.io/jq/)
and
[jsonlint](https://www.npmjs.com/package/jsonlint)
should be used.


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

Imagine you have a messy/packed JSON file that you would like to make readable.

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

### Recursion

This tool **will not** recurse directories. To perform such a task it is best to
use `find` and pipe it to `xargs json-fixer`;

```
find /path/to/recurse -name "*.json" | xargs json-fixer
```

### Errors

If you try and fix a file which does not contain JSON, or is invalid JSON, you
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

Please note that the number in brackets is a JSON decoding error code, and does
not relate to a line number or any other information which would be useful in
debugging the issue. Please use the [jq](https://stedolan.github.io/jq/)
command, or similar command, for ascertaining such debugging information.
