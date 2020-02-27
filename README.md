#### What is this? 
This is a command line tool for environment variables substitution. There is also support for default values, something the regular envsubst (of the operating system) does not have.

It's main usage is substituting environment variables in property files. This is useful in docker entry point files.

This code is based on: https://github.com/a8m/envsubst
The code has been refactored for building just the command line tool and generating smaller binaries.

#### Usage
```sh
envsubst < someproperties.tmpl > someproperties.config
echo 'welcome $HOME ${USER:=defaultuser}' | envsubst
envsubst -help
```

#### Imposing restrictions
There are two command line flags with which you can cause the substitution to stop with an error code, should the restriction associated with the flag not be met. This can be handy if you want to avoid creating e.g. configuration files with unset or empty parameters. The flags and their restrictions are: 

|__Flag__     | __Meaning__    |
| ------------| -------------- |
|`-no-unset`  | fail if a variable is not set
|`-no-empty`  | fail if a variable is set but empty

These flags can be combined to form tighter restrictions. 

### Supported expressions

|__Expression__     | __Meaning__    |
| ----------------- | -------------- |
|`${var}`           | Value of var (same as `$var`)
|`${var-$DEFAULT}`  | If var not set, evaluate expression as $DEFAULT
|`${var:-$DEFAULT}` | If var not set or is empty, evaluate expression as $DEFAULT
|`${var=$DEFAULT}`  | If var not set, evaluate expression as $DEFAULT
|`${var:=$DEFAULT}` | If var not set or is empty, evaluate expression as $DEFAULT
|`${var+$OTHER}`    | If var set, evaluate expression as $OTHER, otherwise as empty string
|`${var:+$OTHER}`   | If var set, evaluate expression as $OTHER, otherwise as empty string
|`$$var`            | Escape expressions. Result will be `$var`. 

<sub>Most of the rows in this table were taken from [here](http://www.tldp.org/LDP/abs/html/refcards.html#AEN22728)</sub>

#### License
MIT
