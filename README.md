# cnAsset
PHP-класс для реализации автоподключения js и css файлов из указанной папки

**Сделано для облегчения работы верстальщика и программиста.**

Класс предназначен для работы с ядром D7 Bitrix, но можно использовать и без битрикса.

При использовании с bitrix файлы подключаются через стандартные функции D7 битрикса `addCss` и `addJs`.



## Использование

Класс принимает два параметра:
1. Массив с путями папок, содержащих js и css файлы (вложенные папки не обрабатываются)
2. [опционально] Массив с префиксами имён файлов для исключения из автоподгрузки. К указанному массиву всегда прибавляются префиксы `-` и `_`.

## Подключение в bitrix

Прежде всего стоит оговориться, что класс имеет смысл использовать только тогда, когда вы ведёте ~~нормальную разработку~~ разработку в папке local примерно так:
```php
// header.php
...

// Заменяем $APPLICATION->showHead() на нормальный вариант, где мы сами можем управлять порядком вывода данных.
// Объяснять это нет смысла, кто использует — точно знает для чего это.
$APPLICATION->ShowMeta("keywords");
$APPLICATION->ShowMeta("description");
$APPLICATION->ShowMeta("robots");

cnAsset::add(array('/local/js/', '/local/css/'), array('main'));

use \Bitrix\Main\Page\Asset;
Asset::getInstance()->addJs('/local/js/main.js', true);

$APPLICATION->ShowCSS();
$APPLICATION->ShowHeadStrings();
$APPLICATION->ShowHeadScripts();

...

```

Положить скрипт в папку /local/php_interface/

Подключить в init.php: `require_once ('cnAsset.php');`

В нужном месте **header.php** шаблона сайта пишем:
```php
cnAsset::add(
	array(
		// Массив с папками, из которых будем тянуть скрипты и стили.
		// вложенные папки не сканируются.
		'/local/js/', 
		'/local/css/'
	), 
	array(
		// Массив с префиксами файлов, исключаемых из автозагрузки.
		// К переданному массиву прибавляются префиксы '-' и '_' .
		// В данном случаи из автоподргузки будут исключены файлы, 
		// имя которых начинается на `main`
		'main'
	)
);
```

Можно ещё вот так:
```php
cnAsset::add(array('/local/css/'));
cnAsset::add(array('/local/js/'));
```


## Подключение без битрикса

Использование без битрикса ничем не отличается от использования с битриксом. Подключаем файл и прописываем нужные параметры. 
```php
require_once ('cnAsset.php');
cnAsset::add(array('/css/'));
cnAsset::add(array('/js/'), array('main'));
```