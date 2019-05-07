# json-fixer
Takes in a json file, and if it's valid will reformat it.

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
