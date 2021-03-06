---
layout: post
title: "Использование связанных пространственных данных в геоинформационных системах"
---

{% section %}
В данной работе описывается расширение QGIS{% sidenote qgis 'QGIS (Quantum GIS) — свободная кроссплатформенная геоинформационная система.' %} для импортирования в проект географической и атрибутивной информации с помощью выполнения SPARQL запросов к хранилищам триплетов RDF.
{% endsection %}

{% section %}
## Введение

Семантическая паутина упрощает задачи интеграции данных, предоставляя инфраструктуру, основанную на RDF и онтологиях. Для того, чтобы использовать паутину как средство интеграции данных, требуются  всеобъемлющие массивы данных и словари, позволяющие устранять неоднозначность и упорядочивать остальную информацию. Например, благодаря проекту DBpedia{% sidenote dbpedia 'DBpedia — краудсорсинговый проект, направленный на извлечение структурированной информации из данных, созданных в рамках проекта Википедия и публикации её в виде доступных под свободной лицензией наборов данных.' %} доступен большой массив данных, предоставляющий энциклопедические знания во множестве различных областей.

OpenStreetMap  -- это некоммерческий проект по созданию подробной, свободной и бесплатной географической карты мира. Этот проект обладает потенциальной возможность стать отправной точкой для интеграции пространственной информации в семантическую паутину.

Целью проекта LinkedGeoData{% sidenote 1 'LinkedGeoData – Collaboratively Created Geo-Information for the Semantic Web / Soren Auer, Jens Lehmann, Sebastian Hellmann, Universitat Leipzig' %}{% sidenote 2 'LinkedGeoData: Adding a Spatial Dimension to the Web of Data / Soren Auer, Jens Lehmann, Sebastian Hellmann, Universitat Leipzig' %}{% sidenote 3 'LinkedGeoData: A Core for a Web of Spatial Open Data / Krzysztof Janowicz, University of California, Santa Barbara, USA' %} является внедрение данных OpenStreetMap в семантическую паутину. Это упростит задачи интеграции и сбора информации из реальной жизни, требующие обширных предварительных познаний о пространственных особенностях объектов реального мира.

Большинство информации получено конвертацией данных из проекта OpenStreetMap в RDF и построения на их основе упрощённой онтологии. Данные LinkedGeoData связанны с массивами данных DBpedia и GeoNames. Разработчики LGD стремятся создать словарь OWL нацеленный на упрощение обмена и повторного использования пространственной информации.

В данной работе описывается способ импортирования географической и атрибутивной информации из хранилища RDF триплетов (например, LinkedGeoData.org) в проекты, создаваемые с помощью QGIS.
{% endsection %}

{% section %}
## Постановка задачи

Для более тесной интеграции геопространственной семантической паутины с существующими геоинформационными системами, необходимо реализовать возможность обмена пространственной и атрибутивной информацией между хранилищами связанных данных и ранее созданными геоинформационными пакетами (ГИП). Первым этапом создания такой возможности является реализация импортирования связанных пространственных данных в геоинформационные системы общего назначения.

Задачей данной работы является создание  расширения для геоинформационной системы общего назначения QGIS, которое позволит выполнять SPARQL запросы к хранилищам RDF триплетов и импортировать полученные данные в проект.
{% endsection %}

{% section %}
## Описание метода

Суть метода заключается в написании пользователем SPARQL запроса, который будучи выполненным, должен вернуть некий набор географически привязанных данных. Каждой записи из набора соответствует поле с геометрической информацией, которое  определяется в процессе синтаксического разбора запроса (используются поля с типом _Geo:Geometry_, и набор полей-атрибутов. Для того, чтобы определить наличие поля с геометрией в наборе данных, используется регулярное выражение из библиотеки OpenLayers для поиска текста в формате Well-known text (WKT).
В итоге, из набора формируется векторный слой, геометрия которого определяется полем с географической привязкой, а атрибутивные данные выбираются из остальных полей набора пользователем.

Отличие этого метода от прямого подключения к реляционной базе пространственных данных OpenStreetMap в возможности использования всех преимуществ, которые предоставляют связанные данные. Например, агрегация данных из распределённых источников.
{% endsection %}

{% section %}
## Обзор расширения

На рис. 1 изображено главное окно расширения.  Пользователь указывает имя нового слоя с данными, адрес SPARQL-endpoint и сам запрос. На рис. 2 векторный точечный слой, полученный в результате выполнения запроса, и его атрибутивная таблица, состоящая из двух полей.

{% maincolumn '/images/linked-geo-data/pic1.png' 'рис. 1. Окно для выполнения SPARQL запроса' %}

{% maincolumn '/images/linked-geo-data/pic2.png' 'рис. 2. Векторный слой и таблица атрибутов' %}

Для быстрого прототипирования и разработки расширения выбран язык Python. Расширение зависит от нескольких библиотек, реализующих необходимую функциональность:

* [RDFExtras](http://pypi.python.org/pypi/rdfextras) --- для синтаксического разбора запроса
* [SPARQL client](http://pypi.python.org/pypi/sparql-client) --- для выполнения запроса
{% endsection %}

{% section %}
## Дальнейшая работа

Расширение реализует базовую функциональность импортирования связанных пространственных данных в геоинформационные пакеты. Необходимо повысить стабильность расширения, переписав его на C++. Это позволит использовать все функциональные возможности Quantum GIS. Необходимо реализовать поддержку геометрий описываемых не только в формате WKT, но и в виде набора полей долготы/широты. Реализовать обработку наборов данных с полями со смешанной геометрией.
{% endsection %}

{% section %}
## Заключение

В данной работе описан метод использования связанных пространственных данных в геоинформационных системах общего назначения.

Применение предложенного метода позволяет сократить время и упростить разработку геоинформационного пакета, благодаря использованию возможностей семантической паутины и связанных данных.

Расширение распространяется под лицензией GNU GPL v2. Исходный код доступен в репозитории [https://github.com/stepankuzmin/lgd](https://github.com/StepanKuzmin/lgd).
{% endsection %}
