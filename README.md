Installation
============

Create folder in root directory you Web-server and clone this project:

    $ cd ~/Sites
    $ git clone https://github.com/livsi/testsuite.behat.git

Write this in you Symfony2 project:
-----------------------------------

In autoload:

    // app/autoload.php
        $loader->registerNamespaces(array(
        // ...
        'Behat\BehatBundle' => '/Users/Standart-User/Sites/testsuite.behat/vendor',
        'Behat\Behat'       => '/Users/Standart-User/Sites/testsuite.behat/vendor/Behat/Behat/src',
        'Behat\Gherkin'     => '/Users/Standart-User/Sites/testsuite.behat/vendor/Behat/Gherkin/src',
        'Behat\Mink'        => '/Users/Standart-User/Sites/testsuite.behat/vendor/Behat/Mink/src',
        'Behat\MinkBundle'  => '/Users/Standart-User/Sites/testsuite.behat/vendor',
        'Goutte'            => '/Users/Standart-User/Sites/testsuite.behat/vendor/Goutte/src',
        'Zend'              => '/Users/Standart-User/Sites/testsuite.behat/vendor/Zend/library',
        'Behat\SahiClient'  => '/Users/Standart-User/Sites/testsuite.behat/vendor/Behat/SahiClient/src',
        'Buzz'              => '/Users/Standart-User/Sites/testsuite.behat/vendor/Buzz/lib',
    // ...
));

In AppKernel.php

    // app/AppKernel.php
        if (in_array($this->getEnvironment(), array('dev', 'test'))) {
        // ...
        $bundles[] = new Behat\BehatBundle\BehatBundle();
        $bundles[] = new Behat\MinkBundle\BehatMinkBundle();
        // ...
        }


In config_dev.yml

    # app/config/config_dev.yml
    framework:
        test:       ~
    
    # ...
    
    behat: ~
    
    behat_mink:
        base_url:  http://your_app_local.url/app_test.php/
        goutte:     ~   # enable both Goutte
        sahi:       ~   # and Sahi session

Init bundle features suite
--------------------------

    app/console behat:bundle --init Acme\\DemoBundle

Test conroller - is optional
----------------------------

    // web/app_test.php
    
    if (!in_array(@$_SERVER['REMOTE_ADDR'], array('127.0.0.1', '::1'))) {
        header('HTTP/1.0 403 Forbidden');
        die('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
    }
    
    require_once __DIR__.'/../app/bootstrap.php.cache';
    require_once __DIR__.'/../app/AppKernel.php';
    
    use Symfony\Component\HttpFoundation\Request;
    
    $kernel = new AppKernel('test', true);
    $kernel->handle(Request::createFromGlobals())->send();
    
Конструкции для тестов:
-----------------------

я на странице "URL"
я перехожу на "URL"
я обновляю страницу "URL"
я перехожу на одну страницу назад
я перехожу на одну страницу вперед
я нажимаю "ИМЯ КНОПКИ"
я кликаю по ссылке "ИМЯ ССЫЛКИ"
я заполняю поле "ИМЯ ПОЛЯ" значением "ЗНАЧЕНИЕ"
я ввожу "ЗНАЧЕНИЕ" в поле "ИМЯ ПОЛЯ"
я ввожу следующие значения:
я выбираю "ЗНАЧЕНИЕ ВЫБОРА" в поле "ИМЯ ПОЛЯ"
я ставлю галочку "ИМЯ ГАЛОЧКИ"
я снимаю галочку "ИМЯ ГАЛОЧКИ"
я выбираю файл "ИМЯ ФАЙЛА" в поле "ИМЯ ПОЛЯ"
я должен видеть "ТЕКСТ"
тело ответа должно содержать "ТЕКСТ"
я не должен видеть "ТЕКСТ"
тело ответа не должно содержать "ТЕКСТ"
поле "ИМЯ ПОЛЯ" должно содержать "ЗНАЧЕНИЕ"
поле "ИМЯ ПОЛЯ" не должно содержать "ЗНАЧЕНИЕ"
галочка "ИМЯ ГАЛОЧКИ" должна быть отмечена
галочка "ИМЯ ГАЛОЧКИ" не должна быть отмечена
я должен быть на странице "URL"
(url|адрес) должен соответствовать "ПАТЕРН"
элемент "ЭЛЕМЕНТ" должен содержать "ЗНАЧЕНИЕ"
я должен видеть "ТЕКСТ" внутри элемента "ЭЛЕМЕНТ"
я должен видеть элемент "ЭЛЕМЕНТ"
я не должен видеть элемент "ЭЛЕМЕНТ"
элемент "ЭЛЕМЕНТ" должен вести на страницу "URL"
элемент "ЭЛЕМЕНТ" должен иметь атрибут "АТРИБУТ", содержащий "ЗНАЧЕНИЕ"
код ответа сервера должен быть "КОД"
выведи последний ответ сервера
покажи последний ответ сервера