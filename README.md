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
<h3>2.4 Установка и настройка python3:</h3>
Для установки трбуется:
<ul>
<li>python3.10 или выше;</li>
<li>выполнить команду pip3 install requirements.txt;</li>
</ul>
<h3>2.5 Загрузка данных</h3>
  <ul>
  <li> Скачиваем данные из <a href='https://www.naturalearthdata.com/http//www.naturalearthdata.com/download/10m/cultural/ne_10m_admin_0_countries.zip'>источника</a></li>
  <li>Разархивируем в geodjango/world/data/</li>
  <li>Выполняем команду orginspect:</li>
   <div>
  
    python3 manage.py orginspect world/data/ne_110m_admin_0_countries.shp ne_110m_admin_0_countries  --srid=4326 --maping --multi
  
</div>
  <li>Полученный слоаврь копируем в файл load.py, а новую модель добавляем в файл models.py (в данном случае так не делаем модель уже добавлена)</li>
  Пример файла load.py:
  <div>
    
    from pathlib import Path
    from django.contrib.gis.utils import LayerMapping
    from .models import ne_10m_ocean # импортируем модель

    ne_10m_ocean_mapping = { # Этот слоаврь заменяем на новый
        'scalerank': 'scalerank',
        'featurecla': 'featurecla',
        'min_zoom': 'min_zoom',
        'geom': 'MULTIPOLYGON',
    }




    world_shp = Path(__file__).resolve().parent / 'data/ne_50m_ocean' / 'ne_50m_ocean.shp' # прописываем путь к файл shp


    def run(verbose=True):
        lm = LayerMapping(ne_10m_ocean, world_shp, ne_10m_ocean_mapping, transform=False) # вставляем наши данные модель, путь, словарь
        lm.save(strict=True, verbose=verbose)
    
  </div>
  <li>Создаём файл миграции данных:
  <div>
    
    $ python3 manage.py makemigrations
    
   </div>
  </li>
  <li>Запускаем миграцию:
  <div>
    
    $ python3 manage.py migrate
    
   </div>
  </li>
 <li>Выполняем команду:
  <div>
    
    $ python3 manage.py shell
    
   </div>
  </li>
  <li>Импортируем файл load.py:</li>
  <div>
  
    >>> from world import load
    
  </div>
  <li>Запускаем загрузку данных:</li>
  <div>
  
    >>> load.run()
    
  </div>
 </ul>
# 3 Запуск
python3 manage.py runserver


# Демонстрация
Пример отображения графика Бразильского реала:
<img src="./readme_assets/Screenshot_2023-03-22_09-44-55.png" width="100%">
Пример отображения графика доллара:
<img src="./readme_assets/Screenshot_2023-03-22_09-46-18.png" width="100%">
Страница отправления письма с курсами
<img src="./readme_assets/Screenshot_2023-03-22_09-48-18.png" width="100%">
Результаты планировщика, который располагается на 5555 порту:
<img src="./readme_assets/Screenshot_2023-03-22_09-58-16.png" width="100%">
