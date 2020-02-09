# Ионосферные предвестники землетрясений

Данный репозиторий создан в рамках работы по изучению аномалий в ионосфере, предшествующих землетрясению (ионосферных предвестников землетрясений). Мы предоставляем доступ к обработанному нами набору данных, полученных с наземных ионозондов, и подсчитанным по ним предвестникам землетрясений. 

Данные содержатся только для периодов в 50 дней до и после сильных (магнитуды ⩾ 5) землетрясений, и только для ионозондов, находившихся в зоне подготовки этих землетрясений [[1]](#prep).

## Источники данных

Данные, собранные ионозондами, были взяты с сайта [Национальных центров экологической информации США](https://www.ngdc.noaa.gov/stp/iono/ionogram.html) (NCEI). 

Информация о землетрясениях была взята с сайта [Геологической службы США](https://www.usgs.gov/natural-hazards/earthquake-hazards/earthquakes) (USGS).

## Описание данных
Все данные хранятся в формате ```.csv``` таблиц, с символом ```\t``` в качестве разделителя.

### Данные с ионозондов
Данные, полученные с ионозондов, можно найти в папке ```/NCEI_dataset/```.

Таблица ```sondes_names```  хранит общую информацию о ионозондах и имеет следующую структуру:

| sonde |	lat |	lon |	name |
|-------|-----|-----|------|
| кодовый номер ионозонда | широта, на которой расположен зонд | долгота, на которой расположен зонд | имя региона, в котором располагается зонд | 

В папке ```ionosondes_data/``` для каждого ионозонда существует файл ```{sonde}.csv```, где _sonde_ - кодовый номер зонда. Такой файл хранит данные, собранные ионозондом (49 характеристик ионосферы), и момент времени, когда эти данные были собраны. Каждый файл имеет одинаковый набор атрибутов, приведенный ниже.


| sonde |	year | date |	h |	m | _parameter1_ | ... | _parameter49_ |
|-------|------|------|---|---|--------------|---|---------------|
| кодовый номер ионозонда | год | дата в формате _гггг-мм-дд_ | час | минута | 1ый из 49 показателей | ... | 49ый из 49 показателей | 

### Предвестники землетрясений

В папке ```/earthquake_precursors/``` можно найти подсчитанные нами признаки, которые могут стать предвестниками землетрясений. Функции, позволяющие вычислить данные признаки по данным с ионозондов, расположены в файле ```/scripts/precursors_calculation.ipynb```

```running_avg/``` содержит файлы ```{sonde}_runing_avg.csv```, в которых для каждого ионозонда подсчитано скользящее среднее параметра foF2 за предшествующие 15 дней (в одинаковый момент времени). Как показано в статье _D. Davidenko & S. Pulinets_ [[2]](#mask), перед землетрясением происходит аномальное отклонение текущих значений foF2 от средних, что и является предвестником землетрясения. 

Для каждой временной точки, заданной с помощью (даты, часа, минуты) в таблице содержатся атрибуты:
- ```foF2``` - значение показателя foF2 в текущий момент;
- ```foF2_running_avg``` - скользящее среднее за предыдущие 15 дней в то же время;
- ```foF2_n_days``` - количество дней (из 15ти предшествующих), в которые значение foF2 было известно.

Другой показатель, аномальные значения которого являются предвестником землетрясения, можно найти в папке ```correlation/```. В работе _S. Pulinets_ [[3]](#corr) было показано, что если два ионозонда находятся достаточно близко друг от друга, но при этом один из них попадает в зону подготовки землетрясения, а другой - нет, то корреляциях их показаний (в частности, foF2) снижается. 

В файле ```correlations.csv``` для близко расположенных пар зондов (sonde1, sonde2) и даты подсчитаны следующие характеристики:
- ```foF2_corr``` - корреляция показаний пары ионозондов в текущую дату;	
- ```foF2_n_hours``` - количество часов, за которые доступны показания с обоих зондов;
- ```foF2_total_time_points``` - общее количество временных точек, для которых доступны показания с обоих зондов.

## Список литературы

<a name="prep">[1]</a>  Zubkov S. I. Miachkin V. I. Dobrovolsky, I. P. Estimation of the size of earthquake
preparation zones. Pure and Applied Geophysics, 117(5):1025--1044, 1979.

<a name="mask">[2]</a>  Dmitry Davidenko and Sergey Pulinets. Deterministic variability of the ionosphere before strong (M ⩾  6) earthquakes in the regions of Greece and Italy based on many years of observation (in russian). Geomagnetism and Aeronomy, 59:529–544, January 2019.

<a name="corr">[3]</a> Sergey Pulinets. Ionospheric precursors of earthquakes; recent advances in theory and practical applications. Terrestrial Atmospheric And Oceanic Sciences, 15:445--467, September 2004.
