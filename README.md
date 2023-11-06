# Study Scriber

## Database
### Install Doctrine
```
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```
### Create database (if not created)
```
php bin/console doctrine:database:create
```
### Create an entity class and migrate
```
php bin/console make:entity
```

## Security
### Install
```
composer require symfony/security-bundle
composer require symfony/form
```
Create controller in order to login
```
php bin/console make:controller Login
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```
Edit controller
```
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Http\Authentication\AuthenticationUtils;

class LoginController extends AbstractController
{
    #[Route('/login', name: 'app_login')]
    public function index(AuthenticationUtils $authenticationUtils): Response
    {
        $error = $authenticationUtils->getLastAuthenticationError();
        $lastUsername = $authenticationUtils->getLastUsername();
        return $this->render('login/index.html.twig',[
            'error' => $error,
            'last_username' => $lastUsername
        ]);
    }
}
```
Add template
```
{# templates/login/index.html.twig #}
{% extends 'base.html.twig' %}

{# ... #}

{% block body %}
    {% if error %}
        <div>{{ error.messageKey|trans(error.messageData, 'security') }}</div>
    {% endif %}

    <form action="{{ path('app_login') }}" method="post">
        <label for="username">Email:</label>
        <input type="text" id="username" name="_username" value="{{ last_username }}">

        <label for="password">Password:</label>
        <input type="password" id="password" name="_password">

        {# If you want to control the URL the user is redirected to on success
        <input type="hidden" name="_target_path" value="/account"> #}
        <input type="hidden" name="_csrf_token" value="{{ csrf_token('authenticate') }}">
        <button type="submit">login</button>
    </form>
{% endblock %}
```
### Create registration form
```
composer require symfony/form
composer require validator
composer require symfony/mailer
composer require symfonycasts/verify-email-bundle
php bin/console make:migration
php bin/console doctrine:migrations:migrate
php bin/console make:registration-form
```