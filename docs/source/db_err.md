

## NameError: name 'DBAPIConnection' is not defined

```
(venv) D:\code\mdwsw>python fastapi_aekb.py
Traceback (most recent call last):
  File "fastapi_aekb.py", line 4, in <module>
    from api.v1.api import api_router
  File "D:\code\mdwsw\api\v1\api.py", line 3, in <module>
.......
  File "D:\code\mdwsw\venv\lib\site-packages\sqlalchemy\util\langhelpers.py", line 334, in _exec_code_in_env
    exec(code, env)
  File "<string>", line 1, in <module>
NameError: name 'DBAPIConnection' is not defined

```

查看安装版本

```
(venv) D:\code\mdwsw\venv\Scripts>pip list
Package            Version
------------------ -----------
anyio              3.6.2
cachetools         5.3.0
....
sniffio            1.3.0
SQLAlchemy         2.0.15
...

[notice] A new release of pip is available: 23.1.2 -> 23.3.1

```



```
>>> import sqlalchemy
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
......
    _exec_code_in_env(code, env, fn.__name__),
  File "D:\code\mdwsw\venv\lib\site-packages\sqlalchemy\util\langhelpers.py", line 334, in _exec_code_in_env
    exec(code, env)
  File "<string>", line 1, in <module>
NameError: name 'DBAPIConnection' is not defined

```



```
(venv) D:\mdwsw\venv\Scripts>python -m pip uninstall sqlalchemy
Found existing installation: SQLAlchemy 2.0.15
Uninstalling SQLAlchemy-2.0.15:
  Would remove:
    d:\mdwsw\venv\lib\site-packages\sqlalchemy-2.0.15.dist-info\*
    d:\mdwsw\venv\lib\site-packages\sqlalchemy\*
Proceed (Y/n)? Y
  Successfully uninstalled SQLAlchemy-2.0.15

(venv) D:\mdwsw\venv\Scripts>python -m pip install sqlalchemy
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting sqlalchemy
  Downloading 
  .......
Successfully installed sqlalchemy-2.0.23

```



```
(venv) D:\wsw\venv\Scripts>python
Python 3.8.10 (tags/v3.8.10:3d8993a, May  3 2021, 11:48:03) [MSC v.1928 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlalchemy
>>> exit()

```

