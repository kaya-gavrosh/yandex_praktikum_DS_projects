# Отток клиентов банка

## Данные

Исторические даные о поведении клиентов и расторжении договоров с банком.

## Цель

Построить модель для прогнозирования ухода клиента 

## Детали 

Необходимо также исследовать дисбаланс классов, поскольку клиенты уходят каждый месяц в небольшом количестве.


## Используемые библиотеки:

- pandas
- matplotlib
- numpy
- sklearn

## Выводы

Была построена модель для задачи классификации, которая предсказывает уход клиента из банка. Модель представляет собой случайный лес с гипер параметрами: n_estimators=42, max_depth=7, class_weight='balanced' (учет дисбалланса классов).

Для этого были осуществлены следующие этапы:
1. Задружены данные.
2. Выполнена предобработка данных:
    - Регист названия столбцов изменен на змеиный
    - Заполенены пропуски в столбце `Tenure`
    - Удален столбец. `row_number` (дублирующий индексы строк).
3.Подготовлены признаки:
    - категориальные данные преобразованы в числовые методом прямого кодирования с учетом возможной дамми-ловушки.
    - выделен целевой признак и признаки (без учета столбцов `surname`, `customer_id`).
    - данные разбиты на три группы: обучающую (60%), валидационную (20%) и тестовую (20%) выборки.
    - количественные данные отмасштабированы с помощью z-преобразования.
4. Исследованы баланс классов. 
    - классы находятся в соотношении примерно 1:4, причем отрицательный класс больше.
    - обучены три модели (логической регресси, решающего дерева и случайного леса) без учета балланса. Значение F1-меры составляет от 0,33 у логистической регрессии до 0,55 у случайного леса. (F1-мера должна приближаться к 1). AOC-ROC составляет от 0,67 у логистической регресии до 0,81 у случайного леса. (AOC-ROC случайной модели = 0,5). Случайный лес показывает лучшие значения метрик F1-меры и AOC-ROC.
5. Рассмотрены различне методы борьбы с дисбалансом на примере самой "скоростной" модели логистической регресси и найдены наиболее удачные параметры для некоторых.
6. Сравнивались F1-мера разных моделей при учете дисбалланса. Как и в случае без учета дисбаланса лучший результат показывает случайный лес.
7. Далее обучалась модель случайного леса с изменением гиперпараметров (глубины от 1 до 20, числа деревьев от 10 до 45) и применением различных методов борьбы с дисбалансом. Даипозон измененеия гиперпараметров был подобрано заранее экспериментально. Наилучший результат у модели случайного леса с взвешенными классами.
8. Модель испытана на тестовой выборке. F1-мера = 0.608, AUC-ROC = 0.853.