---
path: /bash-1
date: 2019-08-02T19:29:16.313Z
title: Some handy Bash reminder commands
---
# Bash commands

## cat

Show file numbers

```
cat -n
```

## less

```
less package.json
```

- Spacebar through long files
- `g` to the top of the file
- `shift+g` to the end of the file
- `/searchterm` searches

## mkdir

```
mkdir -p a/b/c
```

Creates directory c and its parent folders a and b

## mv

```
mv lib/* src
```

Moves everything _inside_ lib to src

## cp

```
cp -R src/* lib
```

Copies src contents to lib

## find

```
find /images -name "*.png"
```

- Find all PNG's in the images folder
- Use `iname` to do a case insensitive search
- Search for folder names: `-type d -name`
- Pass `-delete` to immediately delete the find results

## grep

`cat -n package.json | grep js` === `grep -n "js" package.json`

`grep "js" lib/**/*` searches in all sub folders
`grep "js" -C 1 lib/**/*` outputs 1 extra line around each result to give more context

Regex:
`grep -n -e "fn.[gs]et" file.js` Matches fn.get and fn.set

## curl

`curl -i` Include headers
`curl -L` Follow redirect
`curl -Ls` Follow redirect silent, do not print messages etc
`curl -H` Add headers
`curl -X` Change method, ex. `curl -X POST`
`curl -d` Add body to POST
`curl -o` Output response to file

```
curl -H "Authorization: Bearer 124" http://localhost
curl -iL https://nu.nl -o nu.nl.txt
```

Format JSON response from curl (jsome is a npm package):

```
curl https://swapi.co/api/people/1/ | jsome
```

Get status header by returning just the first row of the headers response:

```
curl -ILs https://google.com | head -n 1
```

## scripts

`chmod u+x` reads as "change mode, give user execute permissions"
and is the same as `chmod +x`

```
echo "echo Hello world" > script.sh
ls -l
chmod u+x script.sh
./script.sh
```

Add parameter to script:

script.sh:

```
echo "$1 world"
```

`./script.sh Hey`

to make a script file available, add it to dir which is referenced by \$PATH, for example /usr/local/bin:

`cp my-script.sh /usr/local/bin/my-script`

## variables

```
name=Tom
```

To make `name` available to all child processes: `export name`
To remove the reference: `unset name`

List all variables:

```
env
```

Clone a different branch into a temp dir

```
temp=$(mktemp -f)
git clone --branch $1 $PWD $temp # Clone the given branch into the temp dir inside the current folder
# Now run some tests for example
```

## functions

```
greet() {
    echo "$1 world"
}

greet "Howdy" # Invoke function
greeting=$(greet "Hello") # Store result
```

## exit statuses

`$?` returns the exit status

`0` - success
`1` - common error

From a script or function:
`exit 1` to return an error

`exit` ends the whole script, use `return` in functions

## conditionals

```
if [[ $USER = 'tom' ]]; then
    echo "true"
elif [[ 1 -eq 1 ]];
    echo "the end"
else
    echo "false"
fi
```

- `=` Equals
- `!=` Equals not
- `1 -eq 1` Numeric equation
- `1 -ne 1` Numeric false equation
- `-e filename` Check if file exists
- `-z $var` Check if variable is empty

### ternary

`[[ $USER = 'tom' ]] && echo "true" || echo "false"`

### split a string

`cut -d ' ' -f 2` reads as: cut the string with delimiter ' ' and get the second portion

```
check_status() {
    local status = $(curl -ILs $1 | head -n 1 | cut -d ' ' -f 2)
    if [[ $status -lt 200 ]] || [[ $status -gt 299 ]]; then
        echo "$1 failed with a $status"
        return 1
    else
        echo "$1 succeeded with a $status"
    fi
}

check_status https://www.google.com
```

## pipe

Show all processes running Chrome:

```
ps ax | grep Chrome | less
```

Write standard output (stout) to file:

```
ls -lA > info.txt
```

## other commands

- `wc` - Word count (word, line, character, and byte count)
- `gzip`
