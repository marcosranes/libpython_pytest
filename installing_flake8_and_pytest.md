### ` $ pip install flake8 `
<span style='color:#fff; font-family: Dejavu Sans Mono; font-size: 1.1em;'>- Path: /home/marcosranes/Desktop/libpython_pytest</span>
```shell
Collecting flake8
  Downloading flake8-4.0.1-py2.py3-none-any.whl (64 kB)
Collecting pycodestyle<2.9.0,>=2.8.0
  Downloading pycodestyle-2.8.0-py2.py3-none-any.whl (42 kB)
Collecting pyflakes<2.5.0,>=2.4.0
  Downloading pyflakes-2.4.0-py2.py3-none-any.whl (69 kB)
Collecting mccabe<0.7.0,>=0.6.0
  Using cached mccabe-0.6.1-py2.py3-none-any.whl (8.6 kB)
Installing collected packages: pyflakes, pycodestyle, mccabe, flake8
Successfully installed flake8-4.0.1 mccabe-0.6.1 pycodestyle-2.8.0 pyflakes-2.4.0
```

### ` $ pip freeze `
```
flake8==4.0.1
mccabe==0.6.1
pycodestyle==2.8.0
pyflakes==2.4.0
```

### ` $ echo "# Flake8 Section" >> requirements-dev.txt `

### ` $ pip freeze >> requirements-dev.txt `

### ` $ cat requirements-dev.txt `
```
# Flake8 Section
flake8==4.0.1
mccabe==0.6.1
pycodestyle==2.8.0
pyflakes==2.4.0
```
###
For the linter config we'll need to create a hidden folder named .flake8 in the root of the project, and also the following parameters.  
```shell
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ echo "[flake8]
> max-line-length = 120
> exclude = .venv" >> .flake8
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$
```
### ` $ cat .flake8 `
```shell
[flake8]
max-line-length = 120
exclude = .venv
```
###
Let's create a python package and run flake8 for inconsistencies
### ` $ mkdir python_package && touch python_package/__init__.py `
### ` $ mkdir python_package/py_code `
### ` $ echo "print('Hello, Flake8..') " >> python_package/py_code/welcome.py `
### ` $ python python_package/py_code/welcome.py `
```
Hello, Flake8..
```
```shell
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ echo "def inverse(word: str) -> str:
>     return word[::-1]
> 
> 
> print(inverse('hello python'))
> 
> 
> 
> " >> python_package/py_code/inverse_func.py
```

### ` $ python python_package/py_code/inverse_func.py `
```
nohtyp olleh
```
### ` $ tree python_package `
```
python_package
├── __init__.py
└── py_code
    ├── inverse_func.py
    └── welcome.py

1 directory, 3 files
```
### ` $ flake8 python_package/ `
```shell
python_package/py_code/welcome.py:1:25: W291 trailing whitespace
python_package/py_code/inverse_func.py:8:1: W391 blank line at end of file
```
Don't get messed... both the codes are working fine, but what flake8 complains about is something that isn't according to
the Pep8 standard (i.e. a whitespace in position 25 at the welcome.py file, line 1, and some blank lines at the end of the
inverse_func.py file). So, that is enough for breaking any application which has the flake8 as a dependency. Thus, you're
so far from having your first green build.

So, Let's proceed to fix it...

### ` $ cat python_package/py_code/welcome.py `
```
print('Hello, Flake8..') |
```
### ` $ cat python_package/py_code/welcome.py `
```
print('Hello, Flake8..')
```
Fixed: Removed trailing whitespace
### ` $ flake8 python_package/ `
```
python_package/py_code/inverse_func.py:8:1: W391 blank line at end of file
```
### ` $ cat python_package/py_code/inverse_func.py `
```
def inverse(word: str) -> str:
    return word[::-1]


print(inverse('hello python'))




```

### ` $ cat python_package/py_code/inverse_func.py `
```
def inverse(word: str) -> str:
    return word[::-1]


print(inverse('hello python'))
```
Fixed: Removed extra blank rows. 
### ` $ flake8 python_package/ `
```shell
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ flake8 python_package/
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ 
```
### Other commands


<span style='color:#fff; font-family: Dejavu Sans Mono; font-size: 1.1em;'>- Path: /home/marcosranes/Desktop/libpython_pytest</span>

### ` $ flake8 | grep blank `
```
./python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```

### ` $ flake8 | grep welc `
```
./python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```

### ` $ flake8 $PWD `
```
/home/marcosranes/Desktop/libpython_pytest/python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```
### ` $ flake8 `
```
./python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```
### ` $ flake8 . `
```
./python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```
### ` $ flake8 ../libpython_pytest/ `
```
../libpython_pytest/python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```
### ` $ flake8 ../../Desktop/Django_Dates/ `
```
../../Desktop/Django_Dates/ImportantDates/Dates/date_track/admin.py:1:1: F401 'django.contrib.admin' imported but unused
../../Desktop/Django_Dates/ImportantDates/Dates/date_track/tests.py:1:1: F401 'django.test.TestCase' imported but unused
../../Desktop/Django_Dates/ImportantDates/Dates/date_track/views.py:1:1: F401 'django.shortcuts.render' imported but unused
../../Desktop/Django_Dates/ImportantDates/Dates/date_track/models.py:1:1: F401 'django.db.models' imported but unused
../../Desktop/Django_Dates/ImportantDatesV2/Dates/date_track/admin.py:1:1: F401 'django.contrib.admin' imported but unused
../../Desktop/Django_Dates/ImportantDatesV2/Dates/date_track/tests.py:1:1: F401 'django.test.TestCase' imported but unused
../../Desktop/Django_Dates/ImportantDatesV2/Dates/date_track/views.py:1:1: F401 'django.shortcuts.render' imported but unused
../../Desktop/Django_Dates/ImportantDatesV2/Dates/date_track/models.py:1:1: F401 'django.db.models' imported but unused
```
`Ctrl+c` to quit scanning
```shell
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ flake8 ../../
^C... stopped
(.venv) marcosranes@ubuntu21-vbox:~/Desktop/libpython_pytest$ 
```
###
However, that uses to be more practice, so, just run...

### ` $ flake8 `
<span style='color:#fff; font-family: Dejavu Sans Mono; font-size: 1.1em;'>- Path: /home/marcosranes/Desktop/libpython_pytest</span>
```
./python_package/py_code/welcome.py:2:1: W391 blank line at end of file
```
For searching for inconsistencies into the whole libpython_pytest project.
