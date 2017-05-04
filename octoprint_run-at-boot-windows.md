# Run at boot on windows (tested windows 7)

Assuming you have followed the instructions [HERE](https://github.com/foosel/OctoPrint/wiki/Setup-on-Windows) to install OctoPrint, and you now wish to have it automatically start on boot
and enjoy the ability to restart octoprint easily via the system menu, and don't want to fiddle with pesky exe to service wrappers, have I got a deal for you!


Create a batch file with this as the contents and save it somewhere (I put it in the venv directory along with `activate.bat`):

    schtasks /end /tn octoprint 
    timeout /t 5
    schtasks /run /tn octoprint

See here for more info on schtasks: https://technet.microsoft.com/en-us/library/cc725744(v=ws.11).aspx

timeout /t 5 waits 5 seconds before starting the task back up again to allow the previous instance time to properly close. You may want to increase that value on slower machines.

Start up task scheduler and add octoprint as a new scheduled task, call it "octoprint"

    program/script: c:\path\to\octoprint.exe
    additional arguments: serve
    start in: c:\path\to_where_octoprint_exe_resides

Add another scheduled task called "restartoctoprint"

    program/script: c:\path\to\restart.bat

The reason there's a separate task to restart, and you can't just call the restart.bat file straight from octoprint is that when the octoprint scheduled task ends, it also prematurely kills the restart batch file.

Finally, open the settings of octoprint and in the "server" section enter this:

Restart OctoPrint: `schtasks /run /tn restartoctoprint`

Restart system: `shutdown /r /t 0`

Shutdown system: `shutdown /s /t 0`
