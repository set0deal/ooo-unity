# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #5 выполнил(а):
- Ваниковский Олег Анатольевич
- НМТ-231801

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

Проекты Unity и скриншот TensorBoard представлены на диске: https://drive.google.com/drive/folders/1cT94LClupJhUVL-vECnEtqcA_W5z28Wi

## Задание 1
### Найдите внутри C# скрипта “коэффициент корреляции ” и сделать выводы о том, как он влияет на обучение модели.

Коэффициент корреляции — это двумерная описательная статистика, количественная мера взаимосвязи (совместной изменчивости) двух переменных. В данной лабораторной работе, коэффициентом корреляции будет максимальное расстояние, на котором агент должен оказаться от цели, в исходном скрипте это 1.42. То есть это то значение, по которому можно соотнести ожидаемый от агента результат и его итог в цикле обучения. Логирование при исходных значениях выглядит так:
![![distance3](![distance3](https://github.com/user-attachments/assets/ca83e799-373b-49a4-930b-60ce807bff5c)
)


Если уменьшить максимальную дистанцию до 1, обучение проходит немного медленнее. До 270000 шага значение среднего вознаграждения намного меньше, то есть агент в меньшем количестве случаев достигает цели. Также увеличивается среднее отклонение от цели. Ко всему прочему, если наблюдать работу обученной с таким значением модели, то будет заметно, что рядом с кубом шарик начинает кружить и выискивать нужную точку, что увеличивает время поиска цели и выглядит не очень хорошо. Скрин логирования при значении максимальной дистанции 1:
![task 1](![distance3](https://github.com/user-attachments/assets/1eb4c055-86b4-4eb4-a984-50388d738257)



Если увеличить максимальную дистанцию до 3, то значение среднего вознаграждения резко возрастает и в течение всего обучения держится очень близко к 1, иногда даже равняется 1. При этом результаты обученной модели не очень хорошие: шар может не достигнуть куба, и при этом будет считаться, что он попал в цель, хотя визуально попадание не прошло, так как объеты были отдалены друг от друга. Показатель логирования при значении максимальной дистанции 1:

![task 1](![distance3 (1)](https://github.com/user-attachments/assets/09a2a3cb-5cff-4fbd-bfb4-67d16f9d2cf3)


Подводя итоги, можно сказать, что коэффициент коррелляции в данном случае будет влиять на то, насколько сильно ML-агент будет стремиться к своей цели и находить идеальную точку попадания, минимизируя среднее отклонение от нее.

Если значение близко к 1, то шар почти в 100% случаев достигает нужной цели. Это означает, что ML-агент обучился по опыту и будет выполнять данные функции. А чем меньше значение, тем хуже агент достигает своей цели, не получая награду за это.

## Задание 2
### Изменить параметры файла yaml-агента и определить какие параметры и как влияют на обучение модели. Привести описание не менее трех параметров.

В качестве изменнения параметров файла, мною был выбран batch_size, то есть размер одного батча был изменен с 10 до 50. Это повлияло ,в большей степени, только на время обучения, которое уменьшилось почти в 2 раза:

![task 2](![batch50](https://github.com/user-attachments/assets/0b9f79d2-bd94-4c5d-9d85-1729369fa4e8)


Следующим параметром стало изменение количества эпох обучения с 3 на 1. Уменьшилось общее время обучения, возросло среднее значение вознаграждения. Значения среднего отклонения колеблются. Следовательно, количество эпох обучения может влиять на финальную точность работы модели, но в текущем случае видимых изменений нет, так как изначально модель не требовала большого количества эпох для обучения.

![task 2](![epochs1](https://github.com/user-attachments/assets/94e20e3f-68d8-4466-9d83-b0a0acaf61fc)


Третий измененный параметр - это максимальное количество шагов при обучении. Обучение завершается в разы быстрее, за счет сокращения кол-ва шагов, но модель начинает работать в разы медленнее - при запуске ML-агент тратит время на поиск цели, а не совершает движение в сторону объекта. Логирование при таком параметре:

![task 2](![steps20000](https://github.com/user-attachments/assets/79559fe0-6aff-4c2c-9f56-43248127e77b)


## Задание 3
### Приведите примеры, для каких игровых задачи и ситуаций могут использоваться примеры 1 и 2 с ML-Agent’ом. В каких случаях проще использовать ML-агент, а не писать программную реализацию решения? 

ML-агента из 1 примера можно использовать в случае, если имеется постоянно передвигающийся игрок и мобы-npc, которые пытаются найти игрока, например, в каком-нибудь синглплеерном шутере (Alien Shooter). В таком случае для них не будет проблем в том, что игрок перемещается - мобы будут следовать за целью, координаты которой постоянно меняются, особенно если это происходит в 3D пространстве (Или изометрическом пространстве), где присутствуют дополнительные препятствия, которые моб не может преодолеть (Объекты выполняют роль физического препятствия). В таком случае у противника будет цель не только найти игрока, но и сделать это самым оптимальным способом, находя при этом самый короткий путь до игрока из возможных представленных.

ML-агента из 2 примера можно использовать, если имеется npc-персонажи, которые должны выполнять некоторую монотонную работу. Например, это применимо для игр в жанре пошаговой стратегии (Hearthstone), в частности если там присутствуют элементы градостроительства и ресурсного менеджмента (Clash Of Clans; Legends mobile). В таком случае, если игрок переопределяет цель, то персонажу легко перестроиться на новый путь следования, особенно с учетом постоянно меняющегося окружения.

В заключение, ML-агенты следует использовать в тех игровых пространствах, где постоянно меняется состояние окружения. Например, игрок имеет большую возможность для передвижения (большие открытые локации, открытый мир; Minecraft, The Long Dark). Или же когда меняется сам ландшафт локации, как в играх, где игрок сам устанавливает какие-либо постройки на карте (Minecraft). Следовательно, персонажу легко составить приоритеты и найти нужный путь до игрока-цели, будь это постоянно перемещающийся игрок или какая-то конечная статическая цель. Для учета состояний потребуется слишком объемный массив переменных, из-за чего вручную написанный скрипт поведения мобов/npc - будет затруднительно.

## Выводы

В ходе лабораторной работы были обучены две разные модели Ml-агентов: поиск постоянно перемещающейся цели и добыча npc-персонажем золота в руднике (перемещение между статическими целями). Для первого случая было сделано несколько вариаций с разными конфигурациями обучения, были прослежены зависимости итогов от входных параметров.


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
