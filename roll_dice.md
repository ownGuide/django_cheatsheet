## Создание проекта и приложения 

_В примерах ниже используется Git Bash_

1. Создать пустой каталог:

```bash
mkdir my_project
```

2. Открыть каталог в редакторе кода (в примере ниже Visual Studio Code)

```bash
code my_project
```

3. Открыть терминал в редакторе кода (`Ctrl + J` в Visual Studio Code)

4. Создать виртуальное окружение

```bash
python -m venv venv
```

5. Активировать виртуальное окружение

```bash
source ./venv/Scripts/activate
```

6. Установить Django

```
pip install django
```

7. Создать новый Django проект

```
django-admin startproject my_project .
```

_точка в конце строки означает создание проекта в текущем каталоге_

_приведенные ниже команды выполняются в корневом каталоге проекта (там, где расположен файл `manage.py`)_


8. Выполнить "начальные" миграции  
_при этом в базе данных будут созданы таблицы, необходимые для работы встроенных в Django "стандартных" приложений_

```bash
python manage.py migrate
```

9.  Создать приложение

```bash
python manage.py startapp my_app
```

10. В файле `settings.py` в каталоге `my_project` добавить строку `'my_app'` в список `INSTALLED_APPS` 

<br>

_фрагмент файла_ `/my_project/settings.py`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'my_app'  # новая строка
]
```

## Страница с результатом броска

11. Создать функцию `home` в файле `views.py`

<br>

_файл_ `/my_app/views.py`
```python
from random import randint

from django.http import HttpResponse


def home(request):
    result = randint(1, 6)
    return HttpResponse(f'Вы бросили кубик. Результат броска: {result}')
```

12. Создать файл `urls.py` в каталоге приложения `my_app`:

<br>

_файл_ `/my_app/urls.py`
```python
from django.urls import path

from .views import home

urlpatterns = [
    path('', home, name='home')
]
```

13. Внести изменения в файл `urls.py` проекта (в каталоге `my_project`)

<br>

_файл_ `/my_project/urls.py`
```python
from django.contrib import admin
from django.urls import include, path  # импорт функции include 

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('my_app.urls'))  # новая строка
]
```

14. Запустить сервер

```bash
python manage.py runserver
```