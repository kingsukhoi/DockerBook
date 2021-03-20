# Debug PHP container with XDebug and PHP Storm

## Debug PHP container with XDebug and PHP Storm

### IMPORTANT: Open docker-compose.yaml in PHPStorm and click the double triangle button next to services 

Port 9000 needs to be free. On windows check by opening PowerShell and running `Get-NetTCPConnection | where Localport -eq 9000 | select Localport,OwningProcess`. XDebug will bind to that port.

**TL;DR MAKE SURE NOTHING IS BOUND TO PORT 9000**

### Instructions

If you use docker.io/kingsukhoi/phpwdebugger container, these are the instructions:

1.Install the XDebug extension for your browser. 

* For Chrome use [https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)
* For Firefox use [https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-for-firefox/](https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-for-firefox/)

2. Open the folder with PHPStorm

3. Start docker-compose. Either

* Open a terminal and run `docker-compose up -d`
* Open docker-compose.yaml in PHPStorm and clikc the double triangle button next to services

![PHPStorm docker-compose](.gitbook/assets/PHPStormDockerCompose.png)

4.. On the top right, click the phone button. This will tell PHPStorm to start listening for connections   

![PHPStorm Debug Button](.gitbook/assets/PHPStormDebugButton.png)

5. Set a break point, click on the space right of the line number  

![PHPStorm Break Point](.gitbook/assets/PHPStormBreakpoint.png)

6. Access the page in the browser. If you are using my docker-compose file, it's localhost:8080. Set the XDebug extension to enabled

![Enable XDebug](.gitbook/assets/xdebugextensionenable.gif)

7.Refresh your php page, and PHPStorm should go into debugging mode. 

![Debug Window](.gitbook/assets/PHPStormDebugWindow.png)

Breakpoints can be set on any file, including files that have been included, and PHP statements in HTML.

