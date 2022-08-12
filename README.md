# The WAP Stack Setup
(Windows, Apache, Python)

## Setup Python
Download and install python 3.9.12
https://www.python.org/ftp/python/3.9.12/python-3.9.12-amd64.exe

Install to:
```
c:\Python39
```

Check version:
```
python --version
```


## Setup Apache
Download Visual C++ 2008 SP1 x64 Redistributable Package
https://aka.ms/vs/17/release/vc_redist.x64.exe

Download Apache 2.4 64bit
https://www.apachelounge.com/download/VS16/binaries/httpd-2.4.54-win64-VS16.zip

Extract

Place Apache24 in C:/

Test by running
```
C:\Apache24\bin\httpd
```

Add to PATH System Variable
```
C:\Apache24\bin
```

Add new system variable
```
MOD_WSGI_APACHE_ROOTDIR = C:\Apache24\
```

Install
```powershell
httpd -k install
```

Start apache
```powershell
httpd -k start
```

## Setup mod_wsgi

Install Microsoft C++ Build Tools
https://aka.ms/vs/17/release/vs_BuildTools.exe

Install mod_wsgi
```powershell
pip install mod_wsgi
```

Run:
```powershell
mod_wsgi-express module-config
```

Output:
```
LoadFile "C:/Python39/python39.dll"
LoadModule wsgi_module "C:/Python39/lib/site-packages/mod_wsgi/server/mod_wsgi.cp39-win_amd64.pyd"
WSGIPythonHome "C:/Python39"
```

Add this code to httpd.conf in apache conf directory:
```apache httpd.conf
# ServerName localhost:80 # use this if you're running this on a VirtualBox VM or PC
ServerName localhost:80


# Django Project
LoadFile "C:/Python39/python39.dll"
LoadModule wsgi_module "C:/Python39/lib/site-packages/mod_wsgi/server/mod_wsgi.cp39-win_amd64.pyd"
WSGIPythonHome "C:/Python39"
WSGIScriptAlias / "C:/www/djagno-test/myproject/myproject/wsgi.py"
WSGIPythonPath "C:/www/djagno-test/myproject/"

<Directory "C:/www/djagno-test/myproject/myproject">
    <Files wsgi.py>
        Require all granted
    </Files>
</Directory>

#Alias /static "C:/www/djagno-test/myproject/static/"
#<Directory "C:/www/djagno-test/myproject/static/">
#    Require all granted
#</Directory>
```

## Setup Django App for Deployment
(this is a short way of doing it, not the propper way)

Edit project/settings.py

```python
# SECURITY WARNING: don't run with debug turned on in production!

DEBUG = False

ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```
