# language-translate-laravel5.4

##You can clone this project

#run command git clone https://github.com/vermaboys/language-translate-laravel5.4.git

#run command composer install

#Install Poedit - https://poedit.net/

#Access using http://localhost/translate/home

##You can also implement language translate in your Laravel project

#Add the composer repository to your composer.json file: "Belphemur/laravel-gettext": "6.x"]

#run composer update. Once it's installed, laravel will discover automatically the provider and load it.]

#You also need to register the LaravelGettext middleware in the app/Http/Kernel.php file:
	
```protected $middlewareGroups = [
	    'web' => [
	      \Xinax\LaravelGettext\Middleware\GettextMiddleware::class,
	   ],
```]
#Be sure to add the line after Illuminate\Session\Middleware\StartSession, otherwise the locale won't be saved into the session]
#you can register the service provider in config/app.php in the providers array:
	
```'providers' = [
        Xinax\LaravelGettext\LaravelGettextServiceProvider::class,
```]

#Now you need to publish the configuration file in order to set your own application values: php artisan vendor:publish

#This command creates the package configuration file in: config/laravel-gettext.php.

##Configuration

At this time your application has full gettext support. Now you need to set some configuration values in config/laravel-gettext.php.

    /**
     * Default locale: this will be the default for your application all
     * localized strings. Is to be supposed that all strings are written
     * on this language.
     */
    'locale' => 'fr_FR',
    /**
     * Supported locales: An array containing all allowed languages
     */
    'supported-locales' => array(
        'es_ES',
        'en_US',
        'fr_FR',
        'es_AR',
    ),
    /**
     * Default charset encoding.
     */
    'encoding' => 'UTF-8',

Ok, now it's configured. It's time to generate the directory structure and translation files for the first time.

#run command php artisan gettext:create

#With this command the needed directories and files are created on resources/lang/i18n ]

##Route and controller implementation example:

#app/Http/routes.php

Route::get('/lang/{locale?}', ['as'=>'lang','uses'=>'HomeController@changeLang']);

#app/Http/Controllers/HomeController.php

/**
* Changes the current language and returns to previous page
* @return Redirect
*/

```public function changeLang($locale=null)
{
    \LaravelGettext::setLocale($locale);
    return \Redirect::to(\URL::previous());
}
```

#A basic language selector example:

#In config/app.php' Add language'=>array('en_US'=>'English','fr_FR'=>'French')

<!-- 
<ul>
@foreach(Config::get('laravel-gettext.supported-locales') as $locale)
    <li><a href="{{route('lang').'/'.$locale}}">{{config('app.language')[$locale]}}</a></li>
    @endforeach 
</ul>-->
