Php Guides
======



## How to trace/debug a php script 

### Requirements:
* Php 8
* xdebug 3.x
* Google Chrome with xdebug helper extension
* VSCode

### Steps:

1. Download and install visual studio code (my setup)
```
Version: 1.54.3 (user setup)
Commit: 2b9aebd5354a3629c3aba0a5f5df49f43d6689f8
Date: 2021-03-15T10:55:45.459Z
Electron: 11.3.0
Chrome: 87.0.4280.141
Node.js: 12.18.3
V8: 8.7.220.31-electron.0
OS: Windows_NT x64 10.0.17763
```
2. Install into vscode php debug extension by Felix Becker (im using 1.14.12)
3. Download xdebug (i'm using php_xdebug-3.0.3-8.0-vs16-x86_64) DLL and then copy into your php extension directory 
4. Open php.ini and add this lines (modify the path to your own)
```
[PHP_XDEBUG]
zend_extension=c:/php/ext/php_xdebug-3.0.3-8.0-vs16-x86_64.dll

xdebug.mode = debug
xdebug.start_with_request = yes
xdebug.client_port = 9003

```
5. Test your installation by creating test.php with this contents:
```php
<?php
xdebug_info();
?>
```
then run it in your server, output must print xdebug variables for it to be correct.
6. In your vscode, add configuration (Menu-> run -> add configuration) for your php-debug plugin, so it looks like the following
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9003
        }
    ]
}
```
make sure this json file resides in the directory of your scripts
7. Install Xdebug helper extension for chrome, and make sure its enabled.
8. Start your webserver.
9. In VSCode, click a line in your code to enable a breakpoint,  type F5 to execute Xdebug listener, then go to URL where your breakpointed script will be executed. It should break.
