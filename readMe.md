Анимированное увеличение чисел
==============================
Анимированное увеличение чисел (далее счетчики) сейчас можно встретить на многих сайтах, ведь с их помощью можно отобразить числа в более интересной и привлекательно форме.

Данный эффект выглядит, как плавное увеличение числа от 0 до какого-то конечного числа. Я уверен, что вы уже неоднократно встречали подобные счетчики.

Как правило их применяют для вывода какой-либо статистики, например, общего количества пользователей, подписчиков, клиентов и т.д.

В этом уроке я покажу, как такой счетчик вы можете установить на своем сайте.
-----------------------------------------------------------------------------
Для этих целей мы воспользуемся уже готовым плагином, которое называется jquery.spincrement.js

Он и позволит нам создать эффект анимированного увеличения чисел.
Чтобы данный скрипт начал работать, нам необходимо:

### 1. Скачать его с GitHub и подключить между тегами head. При этом, библиотека jQuery, также должна быть подключена, иначе ничего работать не будет.
```bash
<script src="js/jquery-2.1.4.min.js"></script>
<script src="js/jquery.spincrement.js"></script>
```
### 2. Обернуть то число (или числа), которые вы хотите анимированно показать, в какой-нибудь тег и задать класс spincrement.
```bash
<span class="spincrement">4089</span>
```
### 3. Проинициализировать плагин
```bash
$(".spincrement").spincrement();
```
Если вы все сделали правильно, то наши числа начнут анимированно выводиться.

Настройка плагина
-----------------
У данного плагина есть небольшой ряд настроек, с помощью которых вы можете сделать работу счетчика более гибкой.

На примере я показал основные из них со значениями по умолчанию. При необходимости, вы можете поменять любой из параметров.
```bash
$(".spincrement").spincrement({
    from: 0,                // Стартовое число
    to: false,              // Итоговое число. Если false, то число будет браться из элемента  
                            // с классом spincrement, также сюда можно напрямую прописать число.  
                            // При этом оно может быть, как целым, так и с плавающей запятой
    decimalPlaces: 0,       // Сколько знаков оставлять после запятой
    decimalPoint: ".",      // Разделитель десятичной части числа
    thousandSeparator: ",", // Разделитель тыcячных
    duration: 1000          // Продолжительность анимации в миллисекундах
});
```
В целом, у нас уже все хорошо работает, но вот беда, счетчики начинают показывать анимацию сразу же после загрузки страницы, а в реальности может быть так, что блок со счетчиками будет находиться где-то в низу страницы и при таком раскладе, ваш посетитель просто не увидит никакой анимации.

Чтобы этого избежать, подобного рода анимацию нужно запускать не в момент загрузки страницы, а лишь тогда, когда пользователь непосредственно прокрутил страницу до нужного блока и уже увидел его в окне браузера.

Запускаем анимацию в момент прокрутки страницы
Для этих целей я написал небольшой скрипт
```bash
$(document).ready(function(){
    var show = true;
	var countbox = "#counts";
	$(window).on("scroll load resize", function(){
		if(!show) return false;                   // Отменяем показ анимации, если она уже была выполнена
		var w_top = $(window).scrollTop();        // Количество пикселей на которое была прокручена 
		                                          // страница
		var e_top = $(countbox).offset().top;     // Расстояние от блока со счетчиками до верха всего 
                                              // документа
		var w_height = $(window).height();        // Высота окна браузера
		var d_height = $(document).height();      // Высота всего документа
		var e_height = $(countbox).outerHeight(); // Полная высота блока со счетчиками
		if(w_top + 200 >= e_top || w_height + w_top == d_height || e_height + e_top < w_height){
			$(".spincrement").spincrement({
				thousandSeparator: "",
				duration: 1200
			});
			show = false;
		}
	});
});
```
А вот так выглядит сам блок со счетчиками
```bash
<section class="section" id="counts">
	<div class="container clearfix text-center">
		<h3>Lorem ipsum dolor</h3>
		<div class="col-4">
			<div class="circle">
				<span class="spincrement">4089</span>
			</div>
			<p class="label">подписчиков</p>
		</div>
		<div class="col-4">
			<div class="circle">
				<span class="spincrement">146</span>
			</div>
			<p class="label">клиентов</p>
		</div>
		<div class="col-4">
			<div class="circle">
				<span class="spincrement">785</span>
			</div>
			<p class="label">партнеров</p>
		</div>
	</div>
</section>
```
Теперь анимация будет запущена только в том случае, если пользователь прокрутил страницу до блока #counts, данный блок в моем случае является неким общим контейнером, в котором находятся три счетчика. Разумеется этих счетчиков может быть сколько угодно.
