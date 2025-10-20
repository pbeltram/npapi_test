## Python scripts

Use Python 3.8.10 or newer.

**Setup for Python 3.9 (VStudio 2019) on Windows 11**

Use pip to install packages on Windows 11.
Pip used `user` directories are located via `%USERPROFILE%` (create if necessary):

```
C:\Users\Administrator\AppData\Roaming\Python\Python39\Scripts
C:\Users\Administrator\AppData\Roaming\Python\Python39\site-packages
```

`%USERPROFILE%` = `C:\Users\Administrator`

Set paths for Python on Windows 11:

Control Panel > System and Security > System and select Advanced system settings
Environment Variables:

System Path variables:

```
"C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64\"
"C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64\Scripts"
```

User Path variables:

```
"%USERPROFILE%\AppData\Roaming\Python\Python39\Scripts"
```

Python upgrade pip package and install needed packages:

```
python.exe -m pip install --user --upgrade pip

python.exe -m pip install --user numpy
python.exe -m pip install --user matplotlib
```
