# geo_project
# 1 Описание
Данный проект предназанчен для отображения карты стран. 

# 2 Установка
Для того, чтобы развернуть данный проект, необходимо настроить геопространственную библиотеку. В данном случае будет использоваться пространственное расширение для PostgreSQL - PostGIS. 
<h3>2.1 Установка Docker:</h3>
<h3>2.2 <a href='https://losst.pro/ustanovka-postgresql-ubuntu-16-04'>Базовая установка Postgres</a></h3> 
<h3>2.3 Установка расширения PostGIS:</h3>
  <ul>
    <li>$ sudo apt-get install postgis</li>
    <li>$ sudo -i -u postgres</li>
    <li>$ psql</li>
    <li>postgres=# create database postgis_db;</li>
    <li>postgres=# create user postgis_user with password 'postgis_user';</li>
    <li>postgres=# grant all on database postgis_db to postgis_user;</li>
    <li>postgres=# create extension postgis; </li>
    <li>postgres=# select postgis_version();</li>
  </ul>
  <div>
    Результат успешной установки пространственного расширения:
    
    postgis_version           
    ---------------------------------------
    3.2 USE_GEOS=1 USE_PROJ=1 USE_STATS=1
    (1 row)
  </div>



Для установки трбуется:
<ul>
<li>python3.10 или выше;</li>
<li>выполнить команду pip3 install requirements.txt;</li>
</ul>
<h1>Запуск</h1>
Данный проект на текущее время работает в тестовом режиме. Для его запуска необходимо будет открыть 3 терминала, в которых запускаются соответствующие команды:
<ul>
<li>1. python3 manage.py runserver</li>
<li>2. celery worker --beat -A curs --loglevel=info</li>
<li>3. flower --beat -A curs --port=5555</li>
</ul>
# Демонстрация
Пример отображения графика Бразильского реала:
<img src="./readme_assets/Screenshot_2023-03-22_09-44-55.png" width="100%">
Пример отображения графика доллара:
<img src="./readme_assets/Screenshot_2023-03-22_09-46-18.png" width="100%">
Страница отправления письма с курсами
<img src="./readme_assets/Screenshot_2023-03-22_09-48-18.png" width="100%">
Результаты планировщика, который располагается на 5555 порту:
<img src="./readme_assets/Screenshot_2023-03-22_09-58-16.png" width="100%">
