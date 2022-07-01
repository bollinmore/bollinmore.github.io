# PyInstaller - A useful to create Windows executable writting by Python.
>Update：  2022/6/23  

It's painful when we want to distribute our python tools to other environment, we cannot make sure the Python version is the same with our machine, and the dependencies are also need to be installed in order to run perfectly, the following are several useful commands when creating Windows executable.

**Basic command**:  
> pyinstaller -F .\app.py  
It will generate an executable called app.exe

**Want to append data?**:  
> pyinstaller -F .\app.py --add-data "<src>;<dst>"
If you want to append your data to the binary, add `--add-data` to the parameter and:  
* <src> is the source path in your current project.
* <dst> is the path where it will extract to.

