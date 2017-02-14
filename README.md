NVDPhpcsStandard
================

Requirements
------------

PHP\_CodeSniffer requires PHP version 5.1.2 or greater, although individual sniffs may have additional requirements such as external applications and scripts. See the [Configuration Options manual page](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Configuration-Options) for a list of these requirements.

The SVN pre-commit hook requires PHP version 5.2.4 or greater due to its use of the vertical whitespace character.

Installation
------------

- You can install PHP\_CodeSniffer using the PEAR installer. This will make the `phpcs` and `phpcbf` commands immediately available for use. To install PHP\_CodeSniffer using the PEAR installer, first ensure you have [installed PEAR](http://pear.php.net/manual/en/installation.getting.php) and then run the following command:
     
  ```
  $ pear install PHP_CodeSniffer
  ```

- If you installed PHP\_CodeSniffer you must add NVD folder to your PHP\_CodeSniffer standard folder so run the following command:

  ```
  $ cd ~
  $ mkdir .phpcs
  $ cd .phpcs
  $ git clone git@github.com:NVD-R/NVDPhpStandard.git
  $ sudo ln -s ~/.phpcs/NVDPhpStandard/NVD /usr/share/pear/PHP/CodeSniffer/Standards/NVD
  ```    
Usage
-----
- PHP_CodeSniffer can have multiple coding standards installed to allow a single installation to be used with multiple projects. When checking PHP code, PHP_CodeSniffer can be told which coding standard to use. This is done using the --standard command line argument so you should use [NVD] standard.

  The example below checks the myfile.php file for violations of the [NVD] coding standard (installed above).
  ```
  $ phpcs --standard=NVD /path/to/code/myfile.php
  ```    
- Also you can use this standard in git hooks file.
  The ecample below run git hooks pre-commit for checks your code when you use [git commit] command.
  ```
  $ cd yourProject/.git/hooks
  $ vim pre-commit
  ``` 
  Add below code to the pre-commit file.
  ```
#!/bin/sh

PROJECT=`php -r "echo dirname(dirname(dirname(realpath('$0'))));"`
STAGED_FILES_CMD=`git diff --cached --name-only --diff-filter=ACMR HEAD | grep \\\\.php`

# Determine if a file list is passed
if [ "$#" -eq 1 ]
then
    oIFS=$IFS
    IFS='
    '
    SFILES="$1"
    IFS=$oIFS
fi
SFILES=${SFILES:-$STAGED_FILES_CMD}

echo "Checking PHP Lint..."
for FILE in $SFILES
do
    php -l -d display_errors=0 $PROJECT/$FILE
    if [ $? != 0 ]
    then
        echo "Fix the error before commit."
        exit 1
    fi
    FILES="$FILES $PROJECT/$FILE"
done

if [ "$FILES" != "" ]
then
    echo "Running Code Sniffer..."
    phpcbf --standard=NVD --encoding=utf-8 -n -p $FILES
    phpcs --standard=NVD --encoding=utf-8 -n -p $FILES
    
    if [ $? != 0 ]
    then
        echo "Fix the error before commit."
        exit 1
    fi
fi

exit $?
```

Standard Code For Instance
--------------------------
- The code below is an instance for standard coding.
```
<?php
namespace Instance;

use Framwork\Lib
use Infrastructure\Lib
use Instance\Lib;

class InstanceInstance extends InstanceParrent
{
  const INSTANCE;
  
  private $_instancePrivate;
  
  protected $instanceProtected;
  
  public $instancePublic;
  
  /**
   * Instance optional comment
   *
   * @return void
   */
  public function instanceFunctionOne($var1, $var2)
  {
    if(condition1 && condition2)
    {
      $arrayInstance = ["1", "2"];
    }
  }
  
  public function instanceFunctionTwo(
    $var1,
    $var2,
    $var3,
    $var4
  )
  {
    if(
      condition1 && 
      condition2 &&
      condition3 ||
      confition4
    )
    {
      $arrayInstance = [
                        "1",
                        "2",
                        "3",
                        "4"
                       ];
                        
    }
  }
}
```
