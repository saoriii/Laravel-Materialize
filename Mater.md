# Installer **Materialize** sur son blog Laravel

## **Node.JS** et Packages **Materialize**

Il faut d’abord installer **Node.JS** (c’est un .exe) https://nodejs.org/en/

Il faut prendre la version **8.11.3** LTS

![img](https://i.imgur.com/z5oKTFG.png)
 
Une fois installé, il faut redémarrer son terminal.

Pour installer Node sur son terminal il faut faire les lignes de codes suivantes :

- `npm i` sert à installer la commande npm dans ton terminal
- `npm install materialize-css --save-dev` sert à installer les paquets du framework

On va maintenant mettre à jour le fichier `resources/assets/sass/app.scss` pour importer **materialize**.
Donc dans ce fichier il faut supprimer le contenu et mettre le code suivant :

~~~~CSS
// Fonts
//@import url("https://fonts.googleapis.com/css?family=Raleway:300,400,600");
@import url("https://fonts.googleapis.com/icon?family=Material+Icons");

// Variables
//@import "variables";

// Bootstrap
//@import "~bootstrap-sass/assets/stylesheets/bootstrap";

//Materialize
@import "~materialize-css/sass/materialize.scss";
~~~~

Pour valider et actualiser le tout il faut faire la commande:
 - `npm run watch`



Maintenant tu peux actualiser ton blog sur ta page web et tu devrais avoir un truc qui ressemble à ça : 

![img](https://i.imgur.com/DaBMme9.png)

---

Maintenant tu vas devoir modifier le code de tes différentes ressources pour avoir un visuel structuré et compréhensible.

# Layouts
Dans `resources/views/layouts/app.blade.php` il faut mettre:

~~~~HTML
<!DOCTYPE html>
<html lang="{{ app()->getLocale() }}">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>{{ config('app.name', 'Laravel') }}</title>
    <!-- Styles -->
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">    
    @yield('css')
</head>
<body>
    <div id="app">
        @auth
            <ul id="dropdown1" class="dropdown-content">
                <li>
                    <a href="{{ route('logout') }}"
                        onclick="event.preventDefault();
                                 document.getElementById('logout-form').submit();">
                        Logout
                    </a>
                    <form id="logout-form" action="{{ route('logout') }}" method="POST" style="display: none;">
                        {{ csrf_field() }}
                    </form>
                </li>
            </ul>
        @endauth
        <nav>
            <div class="nav-wrapper">
                <a href="{{ url('/') }}" class="brand-logo">&nbsp{{ config('app.name', 'Laravel') }}</a>
                <a href="#" data-activates="mobile-demo" class="button-collapse"><i class="material-icons">menu</i></a>
                @guest
                    <ul class="right hide-on-med-and-down">
                        <li><a href="{{ route('login') }}">Login</a></li>
                        <li><a href="{{ route('register') }}">Register</a></li>
                    </ul>
                    <ul class="side-nav" id="mobile-demo">
                        <li><a href="{{ route('login') }}">Login</a></li>
                        <li><a href="{{ route('register') }}">Register</a></li>
                    </ul>
                @else
                    <ul class="right hide-on-med-and-down">
                        <li><a class="dropdown-button" href="#!" data-activates="dropdown1">{{ Auth::user()->name }}<i class="material-icons right">arrow_drop_down</i></a></li>
                    </ul>
                    <ul class="right hide-on-med-and-down">
                        <li><a class="dropdown-button" href="#!" data-activates="dropdown1">{{ Auth::user()->name }}<i class="material-icons right">arrow_drop_down</i></a></li>
                    </ul>
                @endguest
            </div>
        </nav>
        @yield('content')
    </div>
    <!-- Scripts -->
    <script src="{{ asset('js/app.js') }}"></script>
    <script>
        $(".button-collapse").sideNav()
        $(".dropdown-button").dropdown()
    </script>
</body>
</html>
~~~~
Du coup maintenant tu devrais avoir un aperçu un peu plus beau et qui ressemble à ça:

![img](https://laravel.sillo.org/wp-content/uploads/2017/10/Capture-150-1024x321.png)


# Login

Dans `resources/views/auth/login.blade.php` il faut mettre:

~~~~HTML
@extends('layouts.app')
@section('css')
    <style>
        .card {
          margin-top: 40px;  
        }
    </style>
@endsection
@section('content')
<div class="container">
    <div class="row">
        <div class="card">
            <form  method="POST" action="{{ route('login') }}"> 
                <div class="card-content">   
                    {{ csrf_field() }}
                    <span class="card-title">Login</span>
                    <hr>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">mail</i>
                            <input id="email" type="email" name="email" value="{{ old('email') }}" class="{{ $errors->has('email') ? 'invalid' : '' }}" required autofocus>
                            <label for="email" data-error="{{ $errors->has('email') ? $errors->first('email'): '' }}">E-Mail Address</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password" type="password" name="password" class="{{ $errors->has('password') ? 'invalid' : '' }}" required>
                            <label for="password" data-error="{{ $errors->has('password') ? $errors->first('password'): '' }}">Password</label>
                        </div>
                    </div>
                    <p>
                        <input type="checkbox" id="remember" {{ old('remember') ? 'checked' : '' }}>
                        <label for="remember">Remember Me</label>
                    </p>
                </div>
                <div class="card-action">
                    <button class="btn waves-effect waves-light" type="submit" name="action">Login
                        <i class="material-icons right">lock_open</i>
                    </button>
                    <a class="waves-effect waves-light btn" href="{{ route('password.request') }}">Forgot Your Password?<i class="material-icons right">message</i></a>
                </div>
            </form>    
        </div>
    </div>
</div>
@endsection
~~~~

# Register

Dans `resources/views/auth/register.blade.php` il faut mettre:

~~~~HTML
@extends('layouts.app')
@section('css')
    <style>
        .card {
          margin-top: 40px;  
        }
    </style>
@endsection
@section('content')
<div class="container">
    <div class="row">
        <div class="card">
            <form  method="POST" action="{{ route('register') }}"> 
                <div class="card-content">   
                    {{ csrf_field() }}
                    <span class="card-title">Register</span>
                    <hr>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">person</i>
                            <input id="name" type="text" name="name" value="{{ old('name') }}" class="{{ $errors->has('name') ? 'invalid' : '' }}" required autofocus>
                            <label for="name" data-error="{{ $errors->has('name') ? $errors->first('name'): '' }}">Name</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">mail</i>
                            <input id="email" type="email" name="email" value="{{ old('email') }}" class="{{ $errors->has('email') ? 'invalid' : '' }}" required autofocus>
                            <label for="email" data-error="{{ $errors->has('email') ? $errors->first('email'): '' }}">E-Mail Address</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password" type="password" name="password" class="{{ $errors->has('password') ? 'invalid' : '' }}" required>
                            <label for="password" data-error="{{ $errors->has('password') ? $errors->first('password'): '' }}">Password</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password-confirm" type="password" name="password_confirmation" required>
                            <label for="password-confirm">Confirm Password</label>
                        </div>
                    </div>
                </div>
                <div class="card-action">
                    <button class="btn waves-effect waves-light" type="submit" name="action">Register
                        <i class="material-icons right">create</i>
                    </button>
                </div>
            </form>    
        </div>
    </div>
</div>
@endsection
~~~~

# Email

Dans `resources/views/auth/passwords/email.blade.php` il faut mettre:

~~~~HTML
@extends('layouts.app')
@section('css')
    <style>
        .row > .card {
          margin-top: 40px;  
        }
    </style>
@endsection
@section('content')
<div class="container">
    <div class="row">
        <div class="card">
            <form  method="POST" action="{{ route('password.email') }}"> 
                <div class="card-content">
                    {{ csrf_field() }}
                    @if (session('status'))
                        <div class="card green darken-1">
                            <div class="card-content white-text">
                                {{ session('status') }}
                            </div>
                        </div>
                    @endif
                    <span class="card-title">Reset Password</span>
                    <hr>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">mail</i>
                            <input id="email" type="email" name="email" value="{{ old('email') }}" class="{{ $errors->has('email') ? 'invalid' : '' }}" required autofocus>
                            <label for="email" data-error="{{ $errors->has('email') ? $errors->first('email'): '' }}">E-Mail Address</label>
                        </div>
                    </div>
                </div>
                <div class="card-action">
                    <button class="btn waves-effect waves-light" type="submit" name="action">Send Password Reset Link
                        <i class="material-icons right">lock_open</i>
                    </button>
                </div>
            </form>    
        </div>
    </div>
</div>
@endsection
~~~~

# Reset

Dans `resources/views/auth/passwords/reset.blade.php` il faut mettre:

~~~~HTML
@extends('layouts.app')
@section('css')
    <style>
        .card {
          margin-top: 40px;  
        }
    </style>
@endsection
@section('content')
<div class="container">
    <div class="row">
        <div class="card">
            <form  method="POST" action="{{ route('password.request') }}"> 
                <div class="card-content">   
                    {{ csrf_field() }}
                    <input type="hidden" name="token" value="{{ $token }}">
                    <span class="card-title">Reset Password</span>
                    <hr>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">mail</i>
                            <input id="email" type="email" name="email" value="{{ old('email') }}" class="{{ $errors->has('email') ? 'invalid' : '' }}" required autofocus>
                            <label for="email" data-error="{{ $errors->has('email') ? $errors->first('email'): '' }}">E-Mail Address</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password" type="password" name="password" class="{{ $errors->has('password') ? 'invalid' : '' }}" required>
                            <label for="password" data-error="{{ $errors->has('password') ? $errors->first('password'): '' }}">Password</label>
                        </div>
                    </div>
                    <div class="row">
                        <div class="input-field col s12">
                            <i class="material-icons prefix">lock</i>
                            <input id="password-confirm" type="password" name="password_confirmation" required>
                            <label for="password-confirm">Confirm Password</label>
                        </div>
                    </div>
                </div>
                <div class="card-action">
                    <button class="btn waves-effect waves-light" type="submit" name="action">Reset Password
                        <i class="material-icons right">lock_open</i>
                    </button>
                </div>
            </form>    
        </div>
    </div>
</div>
@endsection
~~~~

# Home

Dans `resources/views/auth/passwords/reset.blade.php` il faut mettre:

~~~~HTML
@extends('layouts.app')
@section('content')
<div class="container">
  <div class="row">
    <div class="col s12 m6"> 
    @if (session('status'))
        <div class="card green darken-1">
            <div class="card-content white-text">
                {{ session('status') }}
            </div>
        </div>
    @endif
      <div class="card red lighten-2">
        <div class="card-content white-text">
            <span class="card-title">Dashboard</span>
            You are logged in!
        </div>
      </div>
    </div>
  </div>
</div>
@endsection
~~~~