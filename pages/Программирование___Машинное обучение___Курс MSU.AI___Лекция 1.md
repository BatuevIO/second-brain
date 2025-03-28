- [Ссылка](https://colab.research.google.com/drive/1mk8nlMOfDzJDXfZFjc_H1_WPBiaDOzvA)
- # Базовые задачи
  heading:: 1
	- Классификация -- отнесение образца к одному из попарно непересекающихся множеств. Обучение с учителем.
	  logseq.order-list-type:: number
		- #+BEGIN_EXAMPLE
		  Картинка яблока --> модель --> **"Яблоко"**, "Ананас", "Виноград"
		  #+END_EXAMPLE
	- Регрессия -- нет жесткого ограничения на пространство ответов, пытаемся предсказать число, может даже вещественное. Обучение с учителем.
	  logseq.order-list-type:: number
		- #+BEGIN_EXAMPLE
		  Картинка с 3 яблоками --> модель --> число 3
		  #+END_EXAMPLE
	- Кластеризация -- разбиение множества входных данных на группы. Количество и специфические признаки заранее неизвестны. Тут обучение без учителя!
	  logseq.order-list-type:: number
		- #+BEGIN_EXAMPLE
		  Картинка с кучей фруктов --> модель --> контуры фруктов
		  #+END_EXAMPLE
	- Комбинированные задачи: например, детектирование = классификация + регрессия. 
	  logseq.order-list-type:: number
- # План исследования
  heading:: 1
	- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/pipeline.png)
	- ## Сбор и подготовка данных -- может быть больно, долго и трудоемко. Но это очень важный процесс.
	  heading:: 2
		- Эксперименты в вашей лаборатории
		- [[doc] 🛠️ Соревнования Kaggle](https://www.google.com/url?q=https%3A%2F%2Fwww.kaggle.com%2Fdatasets)
		- [[doc] 🛠️ Google Datasets](https://www.google.com/url?q=https%3A%2F%2Fdatasetsearch.research.google.com%2F)
		- [[article] 🎓 Сайт Papers with Code](https://www.google.com/url?q=https%3A%2F%2Fpaperswithcode.com%2F)
		- Пройтись по похожим/соседним лабораториям, написать авторам статей
		- Важно!
			- Если вы используете данные, скачанные из сети, проверьте, откуда они. Описаны ли они в статье? Если да, посмотрите на документ, убедитесь, что он был опубликован в авторитетном месте, и проверьте, упоминают ли авторы какие-либо ограничения на использованные датасеты.
			- Если данные использовались в ряде работ, это еще не гарантирует высокое качество датасета. **Иногда данные используются только потому, что их легко достать**.
			- Даже широко распространённые датасеты могут иметь ошибки или какую-то странную специфику. Например, при исследовании **ImageNet** были обнаружены миллионы изображений темнокожих людей, которые были помечены как "преступник". В итоге большая часть набора данных ImageNet была удалена.
		- Нужно учитывать особые случаи: например, при обучении все кошки могут быть лысыми, а есть еще сфинксы.
	- ## Разведочный анализ данных (EDA)
	  heading:: 2
		- **Exploratory data analysis**, EDA — анализ основных свойств данных, нахождение в них общих закономерностей, распределений и аномалий, построение начальных моделей с использованием инструментов визуализации.
		- https://habr.com/ru/articles/664102/
		- https://github.com/AleksandrIvchenko/machine-learning-project-walkthrough
	- ## Baseline -- очень простая архитектура, если получается предсказать лучше, чем случайно, то хорошо.
	  heading:: 2
		- **Постройте вашу первую систему быстро**, а затем итерационно улучшайте.
		- Возьмите что-то простое, готовое. Ваша сложная модель должна работать не хуже.
		- Возможно, даже простая модель сможет решить вашу задачу **с достаточным качеством**.
		- **Учтите нижнюю границу качества.** За baseline можно считать известное значение. Например, результат работы классических методов или качество решения задачи человеком.
	- ## Метрики
	  heading:: 2
		- Метрика должна быть выбранной заранее и отвечать целевой задачи.
		- Важно: лучше использовать однопараметрические метрики.
		- Можно учитывать оптимизационные метрики, типа времени работы.
			- Тут можно сделать и однопараметрическую метрику типа F1 + 0.5 * скорость
			- Либо сделать отсечку по времени работы, и целевой метрикой сделать F1 среди оставшихся
	- ## Построение модели, эксперименты
	  heading:: 2
		- TODO лучше начать логгировать результаты экспериментов сразу.
	- ## Проверка гипотез
	  heading:: 2
		- Смотрите на примеры из валидационной выборки, на которых есть ошибки. Так, разумным будет выделить 2 группы объектов:
			- на которых ошибка максимальна,
				- на которых возникают пограничные ошибки.
		- Возьмите разумное количество объектов, которое можно проверить вручную (скажем, 100). Возможно, вы найдёте в этот момент ошибки в разметке или собак, которые очень похожи на котиков.
		- Результат анализа позволит понять, какой ожидаемый эффект будет от дальнейших действий. Если у вас окажется проблема с разметкой, улучшение алгоритма даст малый вклад.
		- Во время улучшения решения у вас будут появляться гипотезы, как можно улучшать решение. Имеет смысл при анализе ошибок завести подобную таблицу, в которой отмечать, на какие объекты в анализируемой подвыборке ожидается эффект.
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/metric4.png){:height 180, :width 687}
		- Таким образом можно оценить первоочередные улучшения.
	- ## Анализ работы модели
	  heading:: 2
		- ![](https://edunet.kea.su/repo/EduNet-web_dependencies/dev-2.4/L01/grad_cam.webp)
		- https://habr.com/ru/articles/419757/
- # Инструменты
  heading:: 1
	- [[doc] 🛠️ NumPy](https://www.google.com/url?q=https%3A%2F%2Fnumpy.org%2F) — поддержка больших многомерных массивов и быстрых математических функций для операций с этими массивами.
	- [[doc] 🛠️ Scikit-learn](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2F) — ML алгоритмы, "toy"-датасеты.
	- [[doc] 🛠️ Pandas](https://www.google.com/url?q=https%3A%2F%2Fpandas.pydata.org%2F) — удобная работа с табличными данными.
	- [[doc] 🛠️ **PyTorch**](https://www.google.com/url?q=https%3A%2F%2Fpytorch.org%2F) — основной фреймворк машинного обучения, который будет использоваться на протяжении всего курса.
	- [[doc] 🛠️ Matplotlib](https://www.google.com/url?q=https%3A%2F%2Fmatplotlib.org%2F) — основная библиотека для визуализации. Вывод различных графиков.
	- [[doc] 🛠️ Seaborn](https://www.google.com/url?q=https%3A%2F%2Fseaborn.pydata.org%2F) — удобная библиотека для визуализации статистик. Прямо из коробки вызываются и гистограммы, и тепловые карты, и визуализация статистик по датасету, и многое другое.
	- ![](https://edunet.kea.su/repo/EduNet-web_dependencies/dev-2.4/L01/sns.png)
	-
- # Данные
  heading:: 1
	- ## Связность данных
	  heading:: 2
		- Существуют различные типы данных с различными типами связности:
			- последовательности (важны порядок данных, время),
			- пространственно-структурированная информация (изображения, видео),
			- табличные данные.
		- Обычно выбор модели под тип данных выглядит следующим образом:
			- **Табличный** — классические ML-модели либо полносвязные NN;
			- **Остальные типы данных** — глубокие нейронные сети.
		- В разных типах данных количество связей между элементами разное и зависит только от типа этих данных. **Важно НЕ количество элементов, а СВЯЗИ между ними.**
		- **Связность** — степень взаимного влияния между соседними элементами.
		  collapsed:: true
			- Например, в таблице, в которой есть определенные параметры (рост, вес), данные между собой связаны, но порядок столбцов значения не имеет. Если мы поменяем столбцы местами, то не потеряем никакой важной информации. Такие данные можно представить в виде вектора, но порядок элементов в нем не важен.
			- При работе с изображениями нам становится важно, как связаны между собой пиксели и по горизонтали, и по вертикали. При добавлении цвета появляются 3 RGB-канала, и значения в каждом канале также связаны между собой. Эту связь нельзя терять, если мы хотим корректно извлечь максимум информации из данных. Соответственно, если дано цветное изображение, то у нас есть уже три измерения, в которых мы должны эти связи учитывать.
	- ## Загрузка и визуализация данных
	  heading:: 2
		- Тут был пример
- # Работа с данными и моделью
  heading:: 1
	- ## Описание модели k-NN
	  heading:: 2
		- Простейшая модель -- k nearest neighbours, k-NN -- метрический алгоритм для классификации или регрессии.
		- Для классификации:
			- Рассматриваются объекты из обучающей выборки, для которых известно, к какому классу они принадлежат.
			- Между подлежащими классификации объектами и объектами тренировочной выборки вычисляется матрица попарных расстояний согласно выбранной метрике.
			- На основе полученной матрицы расстояний для каждого из подлежащих классификации объектов определяются k ближайших объектов тренировочной выборки — k ближайших соседей.
			- Подлежащим классификации объектам приписывается тот класс, который чаще всего встречается у их k ближайших соседей.
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/knn_idea.png)
		- Близость данных согласно метрике. Виды метрик:
			- L_2 -- евклидова метрика на плоскости, корень из суммы квадратов
			- L_1 -- манхэттенская метрика, сумма модулей
			- ang -- угловое расстояние, проекция на круг и арккос
			- Объекты, близкие по одной метрике, не обязательно будут близкими по другой метрике.
	- ## Простейшая метрика
	  heading:: 2
		- Простейшая метрика -- Accuracy = P / N. Она плохо работает, когда есть дисбаланс классов (например, 90% семплов класса 1, и тогда accuracy будет 90%).
	- ## Параметры и гиперпараметры
	  heading:: 2
		- У нас есть два параметра модели, которые можно настраивать:
			- Метрика расстояния
			- Количество ближайших соседей k
		- Настраиваемые параметры называются **гиперпараметрами**. Выбор модели тоже может быть гиперпараметром задачи.
	- ## Разделение train-validation-set
	  heading:: 2
		- Самый простой способ научиться -- "запомнить все". Например, навыку умножать нельзя проверять по таблице умножения, потому что ее можно запомнить. Нужно подсовывать другие примеры, которые модель раньше не видела.
		- Если модель "запомнит всё", то она будет идеально работать на данных, которые мы ей показали, но может вообще не работать на любых других данных.
		- С практической точки зрения важно, как модель будет вести себя именно на незнакомых ей данных, то есть насколько хорошо она научилась обобщать закономерности, которые в данных присутствовали (если они вообще существуют).
		- Для оценки этой способности набор данных разделяют на три части:
			- **Обучающая выборка** (Training set) — выборка данных, которая используется для обучения алгоритма.
			- **Валидационная выборка** (Validation set) — выборка данных, которая используется для подбора параметров, выбора признаков и принятия других решений, касающихся обучения алгоритма.
			- **Тестовая выборка** (Test set) — выборка, которая используется для оценки качества работы алгоритма, при этом никак не используется для обучения алгоритма или подбора используемых при этом обучении параметров.
		- Вполне ожидаемо, оценка по одному соседу оказалась ошибочной. Такие случаи мы называем *переобучением*. Если теперь мы попробуем взять какой-то новый объект и классифицировать его, у нас, скорее всего, ничего не получится. В таких случаях мы говорим, что наша модель не умеет обобщать (*generalization*).
		- Отметим, что качество на тесте получилось заметно выше, чем на валидации. Так произошло потому, что выборки оказались нерепрезентативными, ведь мы делили данные "вслепую", случайным образом. Для того, чтобы выборки были репрезентативными, применяется стратификация.
	- ## Стратификация
	  heading:: 2
		- Метки классов в датасете могут быть распределены неравномерно. Для того, чтобы сохранить соотношение классов при разделении на train и val, необходимо указать параметр `stratify` при разбиении.
		- Еще одним параметром, используемым при разбиении, является `shuffle` (значение по умолчанию `True`). При `shuffle = True` датасет перед разбиением перемешивается.
		- **В случае временных рядов**, текстов и прочих данных, имеющих связь во времени, **данные нельзя перемешивать**. В таких задачах train должен предшествовать val и test по времени. Более подробно об этом будет рассказано в лекции про рекуррентные нейронные сети.
- # Кросс-валидация
  heading:: 1
	- ## Алгоритм кросс-валидации
	  heading:: 2
		- Разбираемся, как подбирать гипер-параметры.
		- Результат работы модели будет зависеть от разбиения. Поэкспериментируем с k-NN и датасетом [Iris 🛠️[doc]](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2Fmodules%2Fgenerated%2Fsklearn.datasets.load_iris.html) и посмотрим, как результат работы модели зависит от `random_state` для `train_test_split`.
		- Результат зависит от того, как нам повезло или не повезло с разбиением данных на обучение и тест. Для одного разбиения хорошо выбрать k=3, а для другого — k=13. Кроме того, фактически мы сами выступаем в роли модели, которая учит гиперпараметры (а не параметры) под видимую ей выборку.
		- Получается, что если подбирать гиперпараметры модели на *train set*, то:
			- Можно переобучитьcя, просто на более "высоком" уровне. Особенно если гиперпараметров у модели много и все они разнообразны.
			- Нельзя быть уверенным, что выбор параметров не зависит от разбиения на обучение и валидацию.
		- Для решения этой проблемы можно произвести **несколько разбиений** датасета на **обучающий и валидационный** по какой-то схеме, чтобы получить уверенность оценок качества для моделей с разными гиперпараметрами.
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/cross_validation_on_train_data.png)
		- Такой подход называется [K-Fold кросс-валидацией 🛠️[doc]](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2Fmodules%2Fcross_validation.html).
		- Берется тренировочная часть датасета и разбивается на части — блоки. Дальше мы будем использовать для проверки первую часть в Fold 1, а на остальных частях будем обучать модель. И так последовательно для всех частей. В результате у нас будет информация о точности для разных фрагментов данных, и уже на основании этого мы сможем понять, насколько значение параметра, который мы проверяем, зависит или не зависит от данных. То есть, если у нас от разбиения точность при одном и том же k меняться не будет, значит, мы подобрали правильный k. Если она будет сильно меняться в зависимости от того, на каком куске данных мы проводим тестирование, значит, надо попробовать другой k, и если ни при каком не получилось, то проблема заключается в данных.
		- Отдельно нужно упомянуть о **временных рядах**. Особенностью таких данных является связность, наличие "настоящего", "прошедшего" и "будущего".
			- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/ts_split.png)
	- ## Оценка результата к-в
	  heading:: 2
		- Тут был пример оценки. Иногда можно пожертвовать точностью (средним) в пользу стабильности (меньшей дисперсии).
		- В идеале -- среднее выше, дисперсия меньше.
	- ## Типичная ошибка к-в
	  heading:: 2
		- **Можно ли делать только кросс-валидацию (без теста)?**
		- Нет, нельзя. Кросс-валидация не до конца спасает от подгона параметров модели под выборку, на которой она проводится. Оценка конечного качества модели должно производиться на отложенной тестовой выборке. Если у вас очень мало данных, можно рассмотреть [вложенную кросс-валидацию 🛠️[doc]](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2Fauto_examples%2Fmodel_selection%2Fplot_nested_cross_validation_iris.html). Речь об этом пойдет в следующих лекциях. Но даже в этом случае придется анализировать поведение модели, чтобы показать, что она учит что-то разумное. Кстати, вложенную кросс-валидацию можно использовать, чтобы просто получить более устойчивую оценку поведения модели на тесте.
	- ## GridSearch
	  heading:: 2
		- Для подбора параметров модели используется **GridSearchCV**.
		- GridSearchCV – это инструмент для автоматического подбора параметров моделей машинного обучения. GridSearchCV находит наилучшие параметры путем обычного перебора: он создает модель для каждой возможной комбинации параметров из заданной сетки.
	- ## RandomizedSearch
	  heading:: 2
		- Альтернативой GridSearch является [RandomizedSearch 🛠️[doc]](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2Fmodules%2Fgenerated%2Fsklearn.model_selection.RandomizedSearchCV.html). Если в GridSearch поиск параметров происходит по фиксированному списку значений, то RandomizedSearch умеет работать с непрерывными значениями, случайно выбирая тестируемые значения, что может привести к более точной настройке гиперпараметров.
		- Вы в явном виде указываете, сколько точек вы будете семплировать.
- # Метрики классификации
  heading:: 1
	- Чтобы создать эффективное решение, важно сначала определить, что мы понимаем под «качеством». Для сравнения различных подходов и выбора лучшего из них необходимо понимать, как правильно измерять их эффективность, и для этого нам нужны соответствующие метрики.
	- ## Accuracy
	  heading:: 2
		- Интуитивно понятной, очевидной и почти неиспользуемой метрикой является уже знакомая нам accuracy — доля правильных ответов алгоритма.
		- **Недостатки метрики accuracy:**
			- **Несбалансированные классы.** Accuracy нельзя использовать, если данные не сбалансированы, то есть в одном из классов больше представителей, чем в другом. На рисунке ниже мы видим, что при явном количественном преобладании объектов класса airplane модель может классифицировать все объекты как airplane и при этом получить такую же точность, как модель, которая качественно разделяет все 3 класса, так как количество ошибок будет равно числу объектов классов, в которых меньше представителей (в данном случае в классах automobile и bird по 10 представителей, соответственно, 20 ошибок).
			  collapsed:: true
				- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/problem_of_simple_way_to_compute_accuracy.png)
			- **Не различает типы ошибок.** Accuracy не учитывает, насколько критичны разные виды ошибок. Ошибки классификации бывают двух видов: **ошибка I-го рода** и **ошибка II-го рода**. Рассмотрим пример теста на беременность. Если женщина действительно беременна, то она принадлежит к классу с меткой 1, иначе — имеет метку 0. Пусть тест на беременность выполняет роль классификатора: показывает беременность (т.е. метку 1) или отсутствие беременности (метку 0).
			  collapsed:: true
				- Таким образом, решающее значение имеет то, какую ошибку выдает классификатор, потому что результат предсказания имеет разные последствия. Например, "не беременна, но тест положительный" соответствует ошибке I-го рода, а "беременна, но тест отрицательный" – ошибке II-го рода.
				- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/1_2_errors.png)
	- ## Confusion matrix
	  heading:: 2
		- Попробуем решить эту проблему. Для этого сначала введём важную концепцию в терминах ошибок классификации — **confusion matrix** (матрицу ошибок). Допустим, у нас есть два класса и алгоритм, предсказывающий принадлежность каждого объекта к одному из классов. Тогда матрица ошибок классификации будет выглядеть следующим образом:
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/conf_matrix.png)
		- Матрица ошибок имеет следующие обозначения:
			- **True Positive (TP):** количество правильных предсказаний для положительного класса.
			- **True Negative (TN):** количество правильных предсказаний для отрицательного класса.
			- **False Positive (FP):** количество неправильных предсказаний, когда отрицательный класс предсказан как положительный.
			- **False Negative (FN):** количество неправильных предсказаний, когда положительный класс предсказан как отрицательный.
		- *Как легко запомнить: True/False — это верное предсказание или нет. Positive/Negative — это какой класс предсказал классификатор. Например, True Positive — предсказание верное (True), классификатор предсказал положительный класс (Positive).*
		- Используя матрицу ошибок, можно рассчитать различные метрики качества классификации, такие как **accuracy**:
		- $$\large\text{Accuracy} = \dfrac{TP + TN}{TP + TN + FP + FN}$$
	- ## Balanced Accuracy
	  heading:: 2
		- В случае дисбаланса классов можно посчитать метрику **balanced accuracy**. Она, в отличие от **accuracy**, учитывает дисбаланс классов:
		- $$\large\text{Balanced Accuracy} = \dfrac{1}{2} (\dfrac{TP}{TP + FN} + \dfrac{TN}{TN + FP})$$
	- ## Precision, Recall
	  heading:: 2
		- Для того, чтобы описать, как качественно алгоритм определяет метку объекта на каждом из классов и как много объектов из имеющихся находит, вводятся метрики **precision (точность)** и **recall (полнота)**.
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/precision-recall.png)
		- $$\large \text{Precision} = \dfrac{TP}{TP + FP}, \quad \text{Recall} = \dfrac{TP}{TP + FN}$$
		- **Именно** введение **precision** не позволяет нам записывать все объекты в один класс, так как в этом случае мы получаем рост уровня False Positive. **Recall демонстрирует способность алгоритма обнаруживать данный класс вообще, а precision — способность отличать этот класс от других классов.**
	- ## F-мера
	  heading:: 2
		- Часто в реальной практике стоит задача найти **оптимальный** **баланс** между **Presicion и Recall**.
		- Для этого вводится **Fβ-мера**:
		- $$\large F_\beta = (1 + \beta^2) \cdot \dfrac{\text{precision} \cdot \text{recall}}{(\beta^2 \cdot \text{precision}) + \text{recall}}$$
		- $\beta$ в данном случае определяет относительное влияние precision и recall в метрике.
		- При $\beta = 1$ $F_\beta$-мера — это среднее гармоническое между Presicion и Recall.
		- $$\large F_1 = \dfrac{2\cdot\text{precision} \cdot \text{recall}}{\text{precision} + \text{recall}}$$
		- F-мера достигает максимума, когда полнота и точность равны единице, и близка к нулю, если один из аргументов близок к нулю.
		- В Sklearn есть удобная функция `sklearn.metrics.classification_report`, возвращающая recall, precision и F1-меру для каждого из классов, а также количество экземпляров каждого класса.
	- ## AUC-ROC
	  heading:: 2
		- Пусть решается задача **бинарной классификации**, и необходимо оценить качество классификатора.
		- В общем случае **классификатор выдаёт предсказания** не в виде "0" и "1", а **в виде вероятности принадлежности к классу "1" в промежутке между 0 и 1**.
		- Для того, чтобы рассчитать все метрики, которые мы рассмотрели выше, требуется бинаризовать предсказания модели по некоторому порогу и построить confusion matrix.
		- Стандартный порог = 0.5. Если вероятность больше порогового значения, объект считается принадлежащим к классу 1, если меньше — к классу 0. Однако порог можно выставить и другой. Для того, чтобы оценить качество модели, не выбирая конкретный порог, можно построить **ROC-кривую**.
		- ### Определение
		  heading:: 3
			- [wiki] 📚 ROC-кривая (Receiver Operating Characteristic curve)](https://www.google.com/url?q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FReceiver_operating_characteristic) — это график изменения True Positive Rate (TPR) против изменения False Positive Rate (FPR) при переборе всех значений порога классификации.
			- TPR и FPR вычисляются по по следующим формулам:
			- TPR=TPTP+FN,FPR=FPFP+TN
			- **Площадь под ROC-кривой** (AUC-ROC, Area Under the ROC Curve) является численной характеристикой качества классификатора.
			- **AUC-ROC является одной из немногих метрик, позволяющих оценить качество классификатора без необходимости подбора конкретного порога.**
			- [[demo] 🎮 Интерактивная визуализация](https://www.google.com/url?q=http%3A%2F%2Fnavan.name%2Froc%2F) влияния порога на разделение двух классов
		- ### Построение
		  heading:: 3
			- Рассмотрим на примере. У нас есть таблица с предсказаниями модели (оценка уверенности в классе 1) и истинной разметкой классов. Отсортируем значения по убыванию оценки от модели. Если классификатор хорошо справляется с задачей, то вверху будет много единиц, а внизу — много нулей. Если классификатор неудачный — случайное распределение меток 0 и 1.
			- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/roc_auc_data_example.png)
			- Приступим непосредственно к изображению графика ROC-кривой. Начнём с квадрата единичной площади и изобразим на нём прямоугольную координатную сетку, равномерно нанеся m горизонтальных линий и n вертикальных. Число горизонтальных линий m соответствует количеству объектов класса 1 из рассматриваемой выборки, а число n — количеству объектов класса 0. В нашем примере m=3 и n=4. Таким образом, квадрат единичной площади разбился на 12 (m×n) прямоугольных блоков.
			- Начиная из точки (0,0), построим ломаную линию в точку (1,1) по узлам получившейся решетки по следующему алгоритму, пройдя по строкам таблицы 2 сверху вниз:
				- **оценка** алгоритма для объекта из текущей строки не равна оценке для объекта из следующей:
					- если класс 1, рисуем линию до следующего узла вертикально вверх
					- если класс 0, рисуем линию до следующего узла горизонтально направо
				- оценки для объектов в нескольких последующих строках совпадают:
					- нарисовать линию из текущего узла в узел, располагающийся на k углов вертикально выше и на l узлов левее. k и l соответственно равны количеству объектов класса 1 и 0 среди группы повторяющихся оценок классификатора
			- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/make_roc_curve.png)
			- **Линия справа** на рис. 1 и есть **ROC-кривая**. Вычислим площадь под получившийся кривой — **AUC-ROC**. AUC-ROC =9.5/12∼0.79.
			- Так как мы начали свое построение с квадрата единичной площади, то AUC-ROC может принимать значения в [0,1].
				- ROC-кривая абсолютно точного бинарного классификатора имеет вид единичного квадрата, его AUC-ROC = 1.
				- Если классификатор для всех объектов предскажет одно и то же значение, то его AUC-ROC = 0.5
				- ROC-кривая для всегда ошибающегося бинарного классификатора, в этом случае AUC-ROC = 0.
	- ## PR-кривая
	  heading:: 2
		- PR-кривая (Precision-Recall Curve) – графическое представление взаимосвязи между precision и recall модели при различных порогах классификации.
		- PR-кривую можно построить с помощью функции `sklearn.metrics.precision_recall_curve` [🛠️[doc]](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2Fmodules%2Fgenerated%2Fsklearn.metrics.precision_recall_curve.html).
		- Она принимает на вход массивы `y_true` и `y_score` и возвращает массивы значений Precision, Recall и перебранных порогов.
	- ## Multiclass accuracy
	  heading:: 2
		- В случае многоклассовой классификации термины TP, FP, TN, FN считаются для каждого класса:
		- ![](https://edunet.kea.su/repo/EduNet-content/dev-2.4/L01/out/confmatrix.png)
		- В случае многоклассовой классификации есть различные способы усреднения результатов, рассмотрим их подробнее на примере расчета метрики Precision:
		- ### Micro-усреднение
		  heading:: 3
			- Сначала объединяем все истинные положительные (TP), ложные положительные (FP) значения по всем классам, а затем считаем метрику на этой общей сумме.
		- ### Macro-усреднение
		  heading:: 3
			- Сначала рассчитываются метрики отдельно для каждого класса, затем берётся среднее арифметическое.
		- ### Weighted-усреднение
		  heading:: 3
			- Классы взвешиваются в зависимости от их размера (количества объектов).
		- ### Samples-усреднение
		  heading:: 3
			- Теперь мы усредняем Precision для всех трёх образцов. В отличие от других методов усреднения (например, Micro или Macro), здесь мы фактически оцениваем предсказания для каждого образца, а затем усредняем результаты, что даёт более точное представление о модели в задаче многозначной классификации.
	- ## Multilabel
	  heading:: 2
		- ![](https://edunet.kea.su/repo/EduNet-web_dependencies/dev-2.4/L01/three_types_of_classification_tasks.jpg)
		- Cлучай, когда объект может принадлежать одновременно нескольким классам, называется *multilabel* (многозначная) классификация. Такую задачу не стоит сводить к задаче бинарной классификации по каждому классу, так как метки могут быть не независимыми.
- # Метрики регресии
  heading:: 1
	- В задаче **регрессии** мы используем входные **признаки**, чтобы предсказать **целевые значения**, являющиеся вещественными числами. Например, можно предсказывать цену жилья по его характеристикам (площадь, этаж, год постройки дома, высота потолков, район, ...).
	- Подробнее про метрики можно почитать [тут 📚[book]](https://www.google.com/url?q=https%3A%2F%2Facademy.yandex.ru%2Fhandbook%2Fml%2Farticle%2Fmetriki-klassifikacii-i-regressii). Там же вы можете найти информацию об относительных ошибках, выражаемых в процентах. Выбор метрики в реальной задаче зависит от традиции, сложившейся в области, поэтому для выбора метрик важно провести литературный обзор.
	- В нашем примере будем использовать уже знакомый алгоритм k-NN, но для регресии. Работает он следующим образом:
		- Запоминаем объекты обучающей выборки.
		- Для нового объекта ищем k ближайших соседей.
		- Предсказание для нового объекта — **это среднее значение целевой переменной k ближайших соседей**.
	- ## MAE (mean absolute error)
	  heading:: 2
		- Чтобы оценить качество модели, посчитаем среднюю абсолютную ошибку $\text{MAE}$:
		  $$\large \text{MAE} = \frac{1}{N} \sum |y_i - f(x_i)|$$
		- **Когда применять:** MAE полезна, когда нам важны простота и интерпретируемость. Она одинаково наказывает все ошибки, без сильного акцента на большие ошибки.
	- ## MSE (mean squared error)
	  heading:: 2
		- Следующая метрика — $\text{MSE}$. Она измеряет среднее значение квадратов ошибок. Из-за возведения ошибок в квадрат большие ошибки наказываются сильнее.
		  $$\large \text{MSE}  = \frac{1}{N} \sum \left(y_i - f(x_i)\right)^2$$
		- **Когда применять:** $\text{MSE}$ полезна, когда нам важно минимизировать крупные ошибки.
	- ## RMSE (root MSE)
	  heading:: 2
		- Чтобы получить оценку ошибки в размерности целевой переменной, можно взять корень (root) от $\text{MSE}$. Это метрика $\text{RMSE}$:
		  $$\large \text{RMSE} = \sqrt{\frac{1}{N} \sum \left(y_i - f(x_i)\right)^2}$$
		- **Когда применять:** $\text{RMSE}$ часто используют в тех же случаях, что и $\text{MSE}$, но она более интерпретируема, так как результат находится в тех же единицах измерения, что и данные.
	- ## R²
	  heading:: 2
		- Существуют и более специфичные метрики, например, $R^2$, которая принимает значения из $(-\infty, 1]$, где $1$  —  наилучший вариант. $R^2$ называется [коэффициентом детерминации 📚[wiki]](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D1%8D%D1%84%D1%84%D0%B8%D1%86%D0%B8%D0%B5%D0%BD%D1%82_%D0%B4%D0%B5%D1%82%D0%B5%D1%80%D0%BC%D0%B8%D0%BD%D0%B0%D1%86%D0%B8%D0%B8) и характеризует долю дисперсии целевого значения, которую объясняет модель.
		  $$\large R^2 = 1 - \frac{\text{MSE}}{\sigma_y^2}=1 - \frac{\sum {\left(y_i-f(x_i)\right)^2}}{\sum{\left(y_i-\bar{y}\right)^2}},$$
		- $$\large \bar{y} = \frac{1}{N}\sum {y_i},$$
		- где $\sigma_y^2$ — дисперсия целевой переменной.
		- **Интуиция:** $R^2$ показывает качество модели в сравнении со случайным предсказателем.
		- **Когда применять:** $R^2$ полезен для оценки общей способности модели объяснять вариацию в данных. Также удобен для сравнения разных моделей.
	- ## MSLE (mean square logarithmic error)
	  heading:: 2
		- В некоторых задачах регрессии ошибки могут иметь разные последствия в зависимости от того, перепредсказали мы или недопредсказали значение. В таких случаях лучше использовать несимметричные метрики, которые учитывают, что одни ошибки хуже других.
		- Например, мы предсказываем количество лекарств на скаладе больницы. Если мы перепредскажем их количество, то останутся излишки, которые могут испортиться. Если мы недопредскажем, то пострадают пациенты. В такой ситуации недопредсказание более критично.
		- $$\large \text{MSLE}  = \frac{1}{N} \sum \left(\ln \left(1+y_i\right) - \ln \left(1+f(x_i)\right)\right)^2$$
		- Полезно для ненормального распредления.
- # Литература
  heading:: 1
	- ## Полезное:
	  heading:: 2
		- [[article] 🎓 Работы выпускников](https://www.google.com/url?q=https%3A%2F%2Fmsu.ai%2Farticles)
		- [[book] 📚 Как писать научные статьи?](https://www.google.com/url?q=https%3A%2F%2Fstepik.org%2Fcourse%2F10524%2Fpromo)
	- ## Данные:
	  heading:: 2
		- [[doc] 🛠️ Соревнования Kaggle](https://www.google.com/url?q=https%3A%2F%2Fwww.kaggle.com%2Fdatasets)
		- [[doc] 🛠️ Google Datasets](https://www.google.com/url?q=https%3A%2F%2Fdatasetsearch.research.google.com%2F)
		- [[article] 🎓 Сайт Papers with Code](https://www.google.com/url?q=https%3A%2F%2Fpaperswithcode.com%2F)
		- [[arxiv] 🎓 Поведение нейросетей и ошибки в разметке](https://www.google.com/url?q=https%3A%2F%2Farxiv.org%2Fabs%2F2211.01866)
		- [[blog] ✏️ Обсуждение проблемы различия данных при обучении и на инференсе](https://www.google.com/url?q=https%3A%2F%2Fstats.stackexchange.com%2Fquestions%2F362906%2Fco-variate-shift-between-train-and-test-data-set)
		- [[doc] 🛠️ Датасет c трафиком из DARPA](https://www.google.com/url?q=https%3A%2F%2Fkdd.ics.uci.edu%2Fdatabases%2Fkddcup99%2Fkddcup99.html)
		- [[doc] 🛠️ CIFAR10](https://www.google.com/url?q=https%3A%2F%2Fwww.cs.toronto.edu%2F%7Ekriz%2Fcifar.html)
	- ## Руководства:
	  heading:: 2
		- [[blog] ✏️ Эндрю Ын. Страсть к Машинному обучению](https://www.google.com/url?q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F419757%2F)
		- [[git] 🐾 Три блокнота с подробным анализом реального датасета](https://github.com/AleksandrIvchenko/machine-learning-project-walkthrough)
		- [[blog] ✏️ Как избежать «подводных камней» машинного обучения: руководство для академических исследователей](https://www.google.com/url?q=https%3A%2F%2Fhabr.com%2Fru%2Fpost%2F664102%2F) — гайд по типичным ошибкам
	- ## Инструменты:
	  heading:: 2
		- [[doc] 🛠️ NumPy](https://www.google.com/url?q=https%3A%2F%2Fnumpy.org%2F) — массивы и математические функции
		- [[doc] 🛠️ Scikit-learn](https://www.google.com/url?q=https%3A%2F%2Fscikit-learn.org%2Fstable%2F) — ML алгоритмы, "toy"-датасеты
		- [[doc] 🛠️ Pandas](https://www.google.com/url?q=https%3A%2F%2Fpandas.pydata.org%2F) — табличные данные
		- [[doc] 🛠️ **PyTorch**](https://www.google.com/url?q=https%3A%2F%2Fpytorch.org%2F) — нейросети
		- [[doc] 🛠️ Matplotlib](https://www.google.com/url?q=https%3A%2F%2Fmatplotlib.org%2F) — визуализация
		- [[doc] 🛠️ Seaborn](https://www.google.com/url?q=https%3A%2F%2Fseaborn.pydata.org%2F) — визуализация статистик
	- ## Методы и алгоритмы:
	  heading:: 2
		- [[wiki] 📚 Метод k-ближайших соседей](https://www.google.com/url?q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FK-nearest_neighbors_algorithm)
		- [[wiki] 📚 Функции расстояния между парой точек](https://www.google.com/url?q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMetric_space)
		- [[git] 🐾 Быстрый k-NN Facebook AI Research Similarity Search](https://github.com/facebookresearch/faiss)
		- [[arxiv] 🎓 Hierarchical Navigable Small World](https://www.google.com/url?q=https%3A%2F%2Farxiv.org%2Fabs%2F1603.09320) — алгоритм поиска ближайших соседей
	- ## Другое:
	  heading:: 2
		- [[blog] ✏️ Цветовые пространства](https://www.google.com/url?q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F181580%2F)
		- [[blog] ✏️ Стратификация](https://www.google.com/url?q=https%3A%2F%2Fwww.reneshbedre.com%2Fblog%2Fstratified-sampling.html)