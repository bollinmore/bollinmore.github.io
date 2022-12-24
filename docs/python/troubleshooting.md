## Windows

1. Cannot run python on Git for Windows - Bash  
*Solution:* Paste the command below in a bash window, then it will work.  
```
echo "alias python='winpty python.exe'" >> ~/.bashrc
. ~/.bashrc
```
