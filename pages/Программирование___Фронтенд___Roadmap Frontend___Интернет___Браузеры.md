- В интересах разработчика оптимизировать свой сайт так, чтобы он загружался быстро и был удобным для пользователя. Поэтому важно понимать, как работают браузеры.
- У браузеров есть только 1 тред -- это важно.
- ## 1 этап -- навигация
  heading:: 2
	- DNS resolve -- он происходит для всех ссылок, которые указаны на странице.
	  logseq.order-list-type:: number
		- Если шрифты лежат на одном домене, картинки на другом, а html на третьем, то будет три DNS резолва.
		  logseq.order-list-type:: number
		- Особенно может повлиять на работу при подключении к мобильной сети (потому что нужно общаться с вышками).
		  logseq.order-list-type:: number
	- TCP handshake -- см. ((67b0bbee-11c4-4360-b5f0-9e20ab818d46))
	  logseq.order-list-type:: number
		- Тоже влияет на скорость взаимодействия -- нужно три сообщения для каждого сервера.
		  logseq.order-list-type:: number
	- TLS negotiation -- см. ((67b0b35f-6e45-45df-af04-47b1123841ae))
	  logseq.order-list-type:: number
		- Еще 5 сообщений.
		  logseq.order-list-type:: number
	- Наконец, реквест -- GET.
	  logseq.order-list-type:: number
- ## 2 этап -- ответ
  heading:: 2
	- Обычно на первый GET запрос приходит ответ с хедерами и html. 
	  logseq.order-list-type:: number
		- На этом же этапе получается первый бит данных. Время от клика на ссылку до получения первого байта называется TTFB (time to first byte).
		  logseq.order-list-type:: number
		- Первый кусок контента обычно весит 14 KB.
		  logseq.order-list-type:: number
	- Congestion control / TCP slow start
	  logseq.order-list-type:: number
		- TCP пакеты делятся на сегменты при транспорте. Т.к. TCP гарантирует последовательность пакетов, сервер должен получить ACK пакет после отправки определенного количества сегментов. 
		  logseq.order-list-type:: number
		- Если сервер будет ждать ACK пакет после каждого сегмента, то это будет долго. 
		  logseq.order-list-type:: number
		- С другой стороны, если посылать слишком много сегментов за раз, то они могут не сразу дойти до клиента, и сервер будет пересылать их несколько раз.
		  logseq.order-list-type:: number
		- Для балансирования нагрузки используется алгоритм TCP Slow Start, где количество передаваемых данных постепенно увеличивается, пока не достигнет предела пропускной способности сети. 
		  logseq.order-list-type:: number
		- Контроль числа передаваемых сегментов -- congestion window (CWND), который может быть задан 1, 2, 4 или 10 MSS (MSS = 1500 байтов через Ethernet). После получения CWND байтов клиент должен отправить ACK пакет. 
		  logseq.order-list-type:: number
		- Если ACK получен, то CWND удваивается для следующей посылки; иначе делится пополам.
		  logseq.order-list-type:: number
- ## 3 этап -- парсинг
  heading:: 2
	- Как только браузер получил первый кусок данных, он может начать его парсить. Парсинг -- процесс перевода полученной информации в DOM и CSSOM, которые затем используются рендерером для отрисовки страницы.
	  logseq.order-list-type:: number
		- DOM -- внутренне отображение маркапа для браузера. Может быть изменен через API с помощью JS.
		  logseq.order-list-type:: number
		- Даже если первая страница HTML будет весить больше 14 KB, браузер все равно попытается отрисовать то, что получил в первом пакете. Поэтому важно включить всю нужную информацию для отрисовки шаблона (HTML и CSS) в первый кусок данных.
		  logseq.order-list-type:: number
	- Очень подробно можно почитать тут: [Critical Rendering Path](https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path) [[Программирование/Фронтенд/Roadmap Frontend/Почитать потом]]
	  logseq.order-list-type:: number
	- Построение DOM-дерева
	  logseq.order-list-type:: number
		- Первый этап -- парсинг HTML и построение DOM-дерева. Информация разбивается на токены, формируются узлы и собираются в дерево. HTML токены имеют начальный и конечный тег, а также имена и значения атрибутов. Хорошая форма документа позволяет быстрее его спарсить. 
		  logseq.order-list-type:: number
		- Первый и корневой тег дерева документа -- <html>. Дерево отражает отношения и иерархию между элементами. Элементы внутри других элементов -- дети. Чем больше элементов, тем дольше парсить.
		  logseq.order-list-type:: number
		- Если парсер нашел неблокирующий ресурс -- например, изображение -- он запросит его и продолжит парсить дальше.
		  logseq.order-list-type:: number
		- Парсинг может продолжиться, если встретился CSS файл.
		  logseq.order-list-type:: number
		- Парсинг останавливается, если встречается тег <script> без async или defer. На самом деле браузеры умеют это предусматривать (preload scanner), но лучше все равно не подключать слишком много скриптов.
		  logseq.order-list-type:: number
		- Preload scanner -- пока браузер строит DOM-дерево (что занимает главный тред), preload scanner парсит весь доступный контент и запрашивает приоритетные ресурсы типа css, js и шрифтов. Поэтому на момент, когда HTML-парсер дойдет до этих тегов, соответствующие ресурсы уже могут быть в пути или на месте.
		  logseq.order-list-type:: number
	- Построение CSSOM-дерева
	  logseq.order-list-type:: number
		- Чем-то похоже на DOM-дерево. Тоже дерево, но они независимы. Браузер переводит CSS правила в карты стилей, с которыми умеет работать.
		  logseq.order-list-type:: number
		- Иерархия в дереве строится на основе CSS-селекторов.
		  logseq.order-list-type:: number
		- Здесь играет роль cascade -- сначала применяются самые общие стили, потом рекурсивно все более и более конкретные.
		  logseq.order-list-type:: number
		- Построение CSSOM-дерева -- очень-очень быстрый процесс, поэтому в девтулзах показывается время Recalculate Style = parse CSS + build CSSOM tree + recursive calculation of computed styles.
		  logseq.order-list-type:: number
	- Другие процессы: компиляция JS
	  logseq.order-list-type:: number
		- Пока идет parsing CSS + CSSOM tree building, идет загрузка других ассетов. JS парсится, компилируется и интерпретируется. Скрипты парсятся в абстрактные синтаксические деревья. [[Программирование/Фронтенд/Roadmap Frontend/Почитать потом]]
		  logseq.order-list-type:: number
		- Некоторые браузеры компилируют эти деревья в байткод. 
		  logseq.order-list-type:: number
		- Большинство кода интерпретируется в главном треде, но есть исключения типа [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API). [[Программирование/Фронтенд/Roadmap Frontend/Почитать потом]]
		  logseq.order-list-type:: number
	- Другие процессы: построение дерева доступности или AOM
	  logseq.order-list-type:: number
		- По сути, AOM это семантическая версия DOM для людей с ограниченными возможностями. 
		  logseq.order-list-type:: number
		- AOM обновляется при обновлении DOM.
		  logseq.order-list-type:: number
		- До построения AOM сайт недоступен для скрин-ридеров.
		  logseq.order-list-type:: number
- ## 4 этап -- рендер
  heading:: 2
	- Рендер состоит из 3-4 частей: стиль, расположение, отрисовка и иногда compositing. DOM и CSSOM комбинируются в дерево рендера, которое используется для вычисления расположения всех видимых элементов. Иногда часть контента может быть вынесена на отдельный слой и отрисована с помощью GPU, что позволяет разгрузить главный тред.
	  logseq.order-list-type:: number
	- Стиль -- построение дерева рендера на основе DOM и CSSOM. 
	  logseq.order-list-type:: number
		- Узлы, которые не будут отрисовываться, в дерево не включаются (типа <head> и display: none).
		  logseq.order-list-type:: number
		- visibility: hidden включаются в дерево
		  logseq.order-list-type:: number
		- Есть user agent default, его можно оверрайдить.
		  logseq.order-list-type:: number
		- К каждому видимому узлу применяются правила CSSOM.
		  logseq.order-list-type:: number
	- Layout/расположение -- вычисление геометрии: размера и местоположения каждого элемента и объекта на странице. Reflow -- любое последующее изменение размера и позиции части или всего документа.
	  logseq.order-list-type:: number
		- Как только дерево рендера построено, запускается layout. Дерево рендера проходится с рута для вычисления геометрии. 
		  logseq.order-list-type:: number
		- Почти все на веб-странице это коробка. Разные устройства и настройки ПК приводят к неограниченному числу разных размеров окна. В этой фазе с учетом размера окна браузер вычисляет все размеры всех прямоугольников на странице. 
		  logseq.order-list-type:: number
		- В основу берется размер окна. Расчет начинается с body и его детей, с учетом параметров бокс-модели и плейсхолдерами для элементов с неизвестными размерами, типа изображений. 
		  logseq.order-list-type:: number
		- Пример reflow -- подгрузилось изображение, стали известны его размеры, и вычисления изменились.
		  logseq.order-list-type:: number
	- Paint/отрисовка -- отрисовка отдельных элементов на экране. Первая отрисовка называется first meaningful paint. На этом этапе происходит перевод размеров и положения коробок в пиксели на экране. Здесь отрисовываются текст, цвета, границы, тени, и замещенные элементы типа кнопок и изображений. Это должно происходит супер быстро. 
	  logseq.order-list-type:: number
		- Для гладкой прокрутки и анимации, все, что занимает главный тред, включая вычисление стилей, должно происходить менее чем за 16.67 мс (60 фпс). 
		  logseq.order-list-type:: number
		- Иногда чтобы ускорить процесс отрисовки, вся страница делится на несколько слоев, но тогда нужен этап композиции. 
		  logseq.order-list-type:: number
		- Теги, которые обозначают слои: video, canvas, и любые с css-свойствами opacity, 3d transform, will change и несколько других. Они и их дети будут отрисованы на отдельных слоях.
		  logseq.order-list-type:: number
		- Слои -- это хорошо, но они едят очень много памяти, так что лучше с ними не перебарщивать.
		  logseq.order-list-type:: number
	- Compositing -- важный этап при работе со слоями. Нужно удостовериться, что они отрисуются в правильной последовательности и корректно. Reflow влечет repaint и recompositing. 
	  logseq.order-list-type:: number
- ## 5 этап -- интерактивность
  heading:: 2
	- Как только главная нить закончила отрисовку страницы, процесс не заканчивается. Например, если был defer файла js после события onload, главная нить может быть занята и недоступна для взаимодействия.
	  logseq.order-list-type:: number
	- Важная метрика -- Time to interactive (TTI). Считается от запроса к DNS lookup до интерактивности (времени после First Contentful Paint, когда страница откликается на взаимодействия в рамках 50 мс)
	  logseq.order-list-type:: number
- ## Ресурсы
  heading:: 2
	- Почитать поподробнее: [тык](https://www.ramotion.com/blog/what-is-web-browser/)
	- Большой видос: [тык]({{video https://www.youtube.com/watch?v=5rLFYtXHo9s}})