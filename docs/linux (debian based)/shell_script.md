# shell script

[explainshell.com](http://explainshell.com)

## general

- To use a variable: ${VARIABLE}, $VARIABLE 
- By convention **variables names** use uppercase snake case (EXAMPLE_VARIABLE). You should not use dash ('-') because it leads to errors when executing the script.
- quotes:
    - single quotes `'` mark literal values. (`echo '$path' #print $path`)
    - In double quotes `"` varriables will be replaced. (`echo "$path" # prints the content of the path variable`)
- To print commands executed in a shell script add `set -o xtrace` to the script or run the script with `bash -x <scriptname.sh>`. Really helpful for debugging.
- to save the return value of a command suraound the command in a dollar and braces e.g `result=$(l -al)`
- if you want to print a variable add quotes around it: `echo "$VARIABLE"`
- you can access the command-arguments/positional-parameter of a script by (incomplete list)
    - `$1`, `$2`, ... (`$0` is the script name) 
    - `$argv[1]`,`$argv[2]`
    - `"$@"` array of all positional parameters, {$1, $2, $3 ...}.
    - `$#`  number of positional parameters.
    - `$$` pid of the current shell (not subshell).
    - `$0` name of the shell or shell script.

## handy

### set relative paths

Sometimes it should be irrelevant from where you start you script. If the script uses relative references to files you first need to find out in which directory the script is saved and add this directory as a prefix to every relative file reference (which makes it an absolute path). Otherwise the working directory of the shell is used as reference.

```shell
#!/bin/bash

# to debug script
set -o xtrace

BASEDIR="$(dirname "$0")"

```

### JSON parsing

- parse a field from a json via grep, awk oder sed
    - `grep -Po '"${JSON_FIELD_NAME}": *"\K[^"]*' ${JSON_FILE_PATH}`
- or better yet use [jq](https://stedolan.github.io/jq/) 

### curl

- To send a json file via curl you need
    - to set content type: ` -H 'Content-Type:application/json'`
    - add the file `-d @RELATIVE/OR/FULLPATH/example.json` or just a string variable `-d @"${CONTENT}`". *Under windows you need to escape the double quotes when using a string variable.*   
    - complete exmaple: `curl -X POST -H 'Content-Type:application/json' -d @RELATIVE/OR/FULLPATH/example.json  $URL`
- to show error but no progress bar run curl: `curl -sS http://`. -s for silent mode (suppress progress bar and error) and -S for explicitly showing  error.

### string replacement

- to replace the first occurrence of a pattern in a string use: `"${stringVarible/patternValue/$replaceValueVariable}"`
- to replace all occurrences of a pattern in a string use: `"${stringVarible//patternValue/$replaceValueVariable}"`
