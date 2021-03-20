# Debug PHP container with XDebug and Visual Studio Code

## Debug PHP container with XDebug and Visual Studio Code

1.Install the XDebug extension for your browser. 

* For Chrome use [https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)
* For Firefox use [https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-for-firefox/](https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-for-firefox/)

2. Install the PHP Debug extention by Felix Becker.  [https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)

![Install Extension](.gitbook/assets/VSCodeInstallXDebug.png)

3. Open a terminal in the current folder, and run `docker-compose up -d`. In VSCode, you can press \```CTRL+ ```  

![Terminal](.gitbook/assets/VSCodeTerminal.png)

4. Create a folder called `.vscode`, and create a file called `launch.json`. In `launch.json` copy and paste the following content. If you are using git, commit this file.

```javascript
{
 "version": "0.2.0",
 "configurations": [
     {
         "name": "Listen for XDebug",
         "type": "php",
         "request": "launch",
         "port": 9000,
         "log": true,
         "pathMappings": {
             "/var/www/html": "${workspaceRoot}"//This references the current directory. If you are using a src folder AND you mounted that folder in docker, add /src to the end of this line.
         }
     },
     {
         "name": "Launch currently open script",
         "type": "php",
         "request": "launch",
         "program": "${file}",
         "cwd": "${fileDirname}",
         "port": 9000
     }
 ]
}
```

5. Set a break point.

![Breakpoint](.gitbook/assets/VSCodeBreakpoint.png)

6. On the left hand toolbar: 1. Click on the triangle with the bug. On the top of that toolbar 2. Make sure the drop-down menu is set to `Listen for XDebug` 3. Press play.  

![Start Debug](.gitbook/assets/VSCodeStartDebug.png)

7. Access the page in the browser. If you are using my docker-compose file, it's localhost:8080. Set the XDebug extension to enabled

![Enable XDebug](.gitbook/assets/xdebugextensionenable.gif)



8. Refresh your php page, and VSCode should go into debugging mode. 

![Debug Window](.gitbook/assets/VSCodeDebugWindow.png)

