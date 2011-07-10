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