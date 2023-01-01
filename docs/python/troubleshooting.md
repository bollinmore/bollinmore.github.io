## Windows

1. Cannot run python on Git for Windows - Bash  
*Solution:* Paste the command below in a bash window, then it will work.  
```
$ echo "alias python='winpty python.exe'" >> ~/.bashrc
. ~/.bashrc
```

2. How to run different python version on Windows?  
*py --list* will list all available python versions on current machine.  
*py -<ver> <cmd>* will run command with specified version.  
```
$ py --list
  *               Active venv
 -V:3.12          Python 3.12 (64-bit)
 -V:3.10          Python 3.10 (64-bit)

$ py -3.10 -m venv .venv
```