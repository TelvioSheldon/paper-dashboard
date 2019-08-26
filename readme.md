# [Paper Dashboard 2 Laravel - Free Frontend Preset for Laravel](https://paper-dashboard-laravel.creative-tim.com/?ref=adnp-readme) [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social&logo=twitter)](https://twitter.com/home?status=Paper%20Dashboard%20Laravel%20is%20a%20Free%20Frontend%20Preset%20for%20Laravel%20%E2%9D%A4%EF%B8%8F%0Ahttps%3A//paper-dashboard-laravel.creative-tim.com/%20%23%paper%20%23design%20%23dashboard%20%23laravel%20%23free%20via%20%40CreativeTim)

<a href="https://packagist.org/packages/laravel-frontend-presets/paper-dashboard"><img src="https://poser.pugx.org/laravel-frontend-presets/paper-dashboard/v/stable.svg" alt="Latest Stable Version"></a> ![license](https://img.shields.io/badge/license-MIT-blue.svg) [![GitHub issues open](https://img.shields.io/github/issues/laravel-frontend-presets/paper-dashboard.svg?maxAge=2592000)](https://github.com/laravel-frontend-presets/paper-dashboard/issues?q=is%3Aopen+is%3Aissue) [![GitHub issues closed](https://img.shields.io/github/issues-closed-raw/laravel-frontend-presets/paper-dashboard.svg?maxAge=2592000)](https://github.com/laravel-frontend-presets/paper-dashboard/issues?q=is%3Aissue+is%3Aclosed)

*Frontend version*: Paper Dashboard 2 v2.1.1. More info at https://www.creative-tim.com/product/paper-dashboard

![Product Image](https://s3.amazonaws.com/creativetim_bucket/products/154/original/opt_md_laravel_thumbnail.jpg?1554814177)

Speed up your web development with the Bootstrap 4 Admin Dashboard built for Laravel Framework 5.5 and up.

## Note

We recommend installing this preset on a project that you are starting from scratch, otherwise your project's design might break.

## Prerequisites

If you don't already have an Apache local environment with PHP and MySQL, use one of the following links:

 - Windows: https://updivision.com/blog/post/beginner-s-guide-to-setting-up-your-local-development-environment-on-windows
 - Linux: https://howtoubuntu.org/how-to-install-lamp-on-ubuntu
 - Mac: https://wpshout.com/quick-guides/how-to-install-mamp-on-your-mac/

Also, you will need to install Composer: https://getcomposer.org/doc/00-intro.md   
And Laravel: https://laravel.com/docs/5.8/installation

## Installation

After initializing a fresh instance of Laravel (and making all the necessary configurations), install the preset using one of the provided methods:

### Via composer

1. `Cd` to your Laravel app  
2. Install this preset via `composer require laravel-frontend-presets/paper-dashboard`. No need to register the service provider. Laravel 5.5 & up can auto detect the package.
3. Run `php artisan preset paper` command to install the Argon preset. This will install all the necessary assets and also the custom auth views, it will also add the auth route in `routes/web.php`
(NOTE: If you run this command several times, be sure to clean up the duplicate Auth entries in routes/web.php)
4. In your terminal run `composer dump-autoload`
5. Run `php artisan migrate --seed` to create basic users table

### By using the archive

1. In your application's root create a **presets** folder
2. [Download an archive](https://github.com/laravel-frontend-presets/paper-dashboard/archive/master.zip) of the repo and unzip it
3. Copy and paste **paper-dashboard-2-master** folder in presets (created in step 2) and rename it to **paper**
4. Open `composer.json` file 
5. Add `"LaravelFrontendPresets\\PaperPreset\\": "presets/paper/src"` to `autoload/psr-4` and to `autoload-dev/psr-4`
6. Add `LaravelFrontendPresets\PaperPreset\PaperPresetServiceProvider::class` to `config/app.php` file
7. In your terminal run `composer dump-autoload`
8. Run `php artisan preset paper` command to install the Paper Dashboard 2 preset. This will install all the necessary assets and also the custom auth views, it will also add the auth route in `routes/web.php`
(NOTE: If you run this command several times, be sure to clean up the duplicate Auth entries in routes/web.php)
9. Run `php artisan migrate --seed` to create basic users table


## Usage

Register a user or login using **admin@paper.com** and **secret** and start testing the preset (make sure to run the migrations and seeders for these credentials to be available).

Besides the dashboard and the auth pages this preset also has a user management example and an edit profile page. All the necessary files (controllers, requests, views) are installed out of the box and all the needed routes are added to `routes/web.php`. Keep in mind that all of the features can be viewed once you login using the credentials provided above or by registering your own user. 

### Dashboard

You can access the dashboard either by using the "**Dashboard**" link in the left sidebar or by adding **/home** in the url. 

### Profile edit

You have the option to edit the current logged in user's profile (change name, email and password). To access this page just click the "**User profile**" link in the left sidebar or by adding **/profile** in the url.

The `App\Htttp\Controlers\ProfileController` handles the update of the user information. 

```
public function update(ProfileRequest $request)
{
    auth()->user()->update($request->all());

    return back()->withStatus(__('Profile successfully updated.'));
}
```

Also you shouldn't worry about entering wrong data in the inputs when editing the profile, validation rules were added to prevent this (see `App\Http\Requests\ProfileRequest`). If you try to change the password you will see that other validation rules were added in `App\Http\Requests\PasswordRequest`. Notice that in this file you have a custom validation rule that can be found in `App\Rules\CurrentPasswordCheckRule`.

```
public function rules()
{
    return [
        'old_password' => ['required', 'min:6', new CurrentPasswordCheckRule],
        'password' => ['required', 'min:6', 'confirmed', 'different:old_password'],
        'password_confirmation' => ['required', 'min:6'],
    ];
}
```

### User management

The preset comes with a user management option out of the box. To access this click the "**User Management**" link in the left sidebar or add **/user** to the url.
The first thing you will see is the listing of the existing users. You can add new ones by clicking the "**Add user**" button (above the table on the right). On the Add user page you will see the form that allows you to do this. All pages are generate using blade templates:

```
<div class="row">
  <label class="col-sm-2 col-form-label">{{ __('Name') }}</label>
  <div class="col-sm-7">
    <div class="form-group{{ $errors->has('name') ? ' has-danger' : '' }}">
      <input class="form-control{{ $errors->has('name') ? ' is-invalid' : '' }}" name="name" id="input-name" type="text" placeholder="{{ __('Name') }}" value="{{ old('name') }}" required="true" aria-required="true"/>
      @if ($errors->has('name'))
        <span id="name-error" class="error text-danger" for="input-name">{{ $errors->first('name') }}</span>
      @endif
    </div>
  </div>
</div>
```

Also validation rules were added so you will know exactely what to enter in the form fields (see `App\Http\Requests\UserRequest`). Note that these validation rules also apply for the user edit option.

```
public function rules()
{
    return [
        'name' => [
            'required', 'min:3'
        ],
        'email' => [
            'required', 'email', Rule::unique((new User)->getTable())->ignore($this->route()->user->id ?? null)
        ],
        'password' => [
            $this->route()->user ? 'nullable' : 'required', 'confirmed', 'min:6'
        ]
    ];
}
```

Once you add more users, the list will get bigger and for every user you will have edit and delete options (access these options by clicking the three dotted menu that appears at the end of every line). 

All the sample code for the user management can be found in `App\Http\Controllers\UserController`. See store method example bellow:

```
public function store(UserRequest $request, User $model)
{
    $model->create($request->merge(['password' => Hash::make($request->get('password'))])->all());

    return redirect()->route('user.index')->withStatus(__('User successfully created.'));
}
```
## Table of Contents

* [Versions](#versions)
* [Demo](#demo)
* [Documentation](#documentation)
* [File Structure](#file-structure)
* [Browser Support](#browser-support)
* [Resources](#resources)
* [Reporting Issues](#reporting-issues)
* [Licensing](#licensing)
* [Useful Links](#useful-links)

## Versions

[<img src="https://github.com/creativetimofficial/public-assets/blob/master/logos/html-logo.jpg?raw=true" width="60" height="60" />](https://demos.creative-tim.com/argon-dashboard-pro/pages/dashboards/dashboard.html?ref=mdl-readme)
[<img src="https://github.com/creativetimofficial/public-assets/blob/master/logos/laravel_logo.png?raw=true" width="60" height="60" />](https://argon-dashboard-pro-laravel.creative-tim.com/?ref=mdl-readme)

| HTML | LARAVEL |
| --- | --- |
| [![Paper Dashboard 2 HTML](https://s3.amazonaws.com/creativetim_bucket/products/50/original/opt_md_thumbnail.jpg?1522232645)](https://demos.creative-tim.com/paper-dashboard/examples/dashboard.html?ref=mdl-readme) | [![Paper Dashboard 2 Laravel](https://s3.amazonaws.com/creativetim_bucket/products/154/original/opt_md_laravel_thumbnail.jpg?1554814177)](https://paper-dashboard-laravel.creative-tim.com/?ref=mdl-readme)

## Demo

| Register | Login | Dashboard |
| --- | --- | ---  |
| [![Register](/screens/Register.png)](https://paper-dashboard-laravel.creative-tim.com/register?ref=mdl-readme)  | [![Login](/screens/Login.png)](https://paper-dashboard-laravel.creative-tim.com/login?ref=mdl-readme)  | [![Dashboard](/screens/Dashboard.png)](https://paper-dashboard-laravel.creative-tim.com/?ref=mdl-readme)

| Profile Page | Users Page | Tables Page  |
| --- | --- | ---  |
| [![Profile Page](screens/Profile.png)](https://paper-dashboard-laravel.creative-tim.com/profile?ref=mdlp-readme)  | [![Users Page](screens/Users.png)](https://paper-dashboard-laravel.creative-tim.com/user?ref=mdl-readme) | [![Tables Page](screens/Tables.png)](https://paper-dashboard-laravel.creative-tim.com/table-list?ref=mdl-readme)
[View More](https://paper-dashboard-laravel.creative-tim.com/?ref=mdl-readme)

## Documentation
The documentation for the Paper Dashboard 2 Laravel is hosted at our [website](https://paper-dashboard-laravel.creative-tim.com/docs/getting-started/laravel-setup.html?ref=mdl-readme).

## File Structure
```
├── app
│   ├── Http
│   │   ├── Controllers
│   │   │   ├── Auth
│   │   │   │   └── RegisterController.php
│   │   │   ├── HomeController.php
│   │   │   ├── PageController.php
│   │   │   ├── ProfileController.php
│   │   │   └── UserController.php
│   │   └── Requests
│   │       ├── PasswordRequest.php
│   │       ├── ProfileRequest.php
│   │       └── UserRequest.php
│   └── Rules
│       └── CurrentPasswordCheckRule.php
├── database
│   └── seeds
│       ├── DatabaseSeeder.php
│       └── UsersTableSeeder.php
└── resources
    ├── assets
    │   ├── css
    │   │   ├── bootstrap.min.css
    │   │   ├── bootstrap.min.css.map
    │   │   ├── paper-dashboard.css
    │   │   ├── paper-dashboard.css.map
    │   │   └── paper-dashboard.min.css
    │   ├── demo
    │   │   ├── demo.css
    │   │   └── demo.js
    │   ├── fonts
    │   │   ├── nucleo-icons.eot
    │   │   ├── nucleo-icons.ttf
    │   │   ├── nucleo-icons.woff
    │   │   └── nucleo-icons.woff2
    │   ├── img
    │   │   ├── apple-icon.png
    │   │   ├── bg
    │   │   │   ├── fabio-mangione.jpg
    │   │   │   └── jan-sendereks.jpg
    │   │   ├── bg5.jpg
    │   │   ├── damir-bosnjak.jpg
    │   │   ├── default-avatar.png
    │   │   ├── faces
    │   │   │   ├── ayo-ogunseinde-1.jpg
    │   │   │   ├── ayo-ogunseinde-2.jpg
    │   │   │   ├── clem-onojeghuo-1.jpg
    │   │   │   ├── clem-onojeghuo-2.jpg
    │   │   │   ├── clem-onojeghuo-3.jpg
    │   │   │   ├── clem-onojeghuo-4.jpg
    │   │   │   ├── erik-lucatero-1.jpg
    │   │   │   ├── erik-lucatero-2.jpg
    │   │   │   ├── joe-gardner-1.jpg
    │   │   │   ├── joe-gardner-2.jpg
    │   │   │   ├── kaci-baum-1.jpg
    │   │   │   └── kaci-baum-2.jpg
    │   │   ├── favicon.png
    │   │   ├── header.jpg
    │   │   ├── jan-sendereks.jpg
    │   │   ├── logo-small.png
    │   │   └── mike.jpg
    │   ├── js
    │   │   ├── core
    │   │   │   ├── bootstrap.min.js
    │   │   │   ├── jquery.min.js
    │   │   │   └── popper.min.js
    │   │   ├── paper-dashboard.js
    │   │   ├── paper-dashboard.js.map
    │   │   ├── paper-dashboard.min.js
    │   │   └── plugins
    │   │       ├── bootstrap-notify.js
    │   │       ├── chartjs.min.js
    │   │       └── perfect-scrollbar.jquery.min.js
    │   └── scss
    │       ├── paper-dashboard
    │       │   ├── _alerts.scss
    │       │   ├── _animated-buttons.scss
    │       │   ├── _buttons.scss
    │       │   ├── cards
    │       │   │   ├── _card-chart.scss
    │       │   │   ├── _card-map.scss
    │       │   │   ├── _card-plain.scss
    │       │   │   ├── _card-stats.scss
    │       │   │   └── _card-user.scss
    │       │   ├── _cards.scss
    │       │   ├── _checkboxes-radio.scss
    │       │   ├── _dropdown.scss
    │       │   ├── _fixed-plugin.scss
    │       │   ├── _footers.scss
    │       │   ├── _images.scss
    │       │   ├── _inputs.scss
    │       │   ├── _misc.scss
    │       │   ├── mixins
    │       │   │   ├── _buttons.scss
    │       │   │   ├── _cards.scss
    │       │   │   ├── _dropdown.scss
    │       │   │   ├── _inputs.scss
    │       │   │   ├── _page-header.scss
    │       │   │   ├── _transparency.scss
    │       │   │   └── _vendor-prefixes.scss
    │       │   ├── _mixins.scss
    │       │   ├── _navbar.scss
    │       │   ├── _nucleo-outline.scss
    │       │   ├── _page-header.scss
    │       │   ├── plugins
    │       │   │   ├── _plugin-animate-bootstrap-notify.scss
    │       │   │   └── _plugin-perfect-scrollbar.scss
    │       │   ├── _responsive.scss
    │       │   ├── _sections.scss
    │       │   ├── _sidebar-and-main-panel.scss
    │       │   ├── _tables.scss
    │       │   ├── _typography.scss
    │       │   └── _variables.scss
    │       └── paper-dashboard.scss
    └── views
        ├── auth
        │   ├── login.blade.php
        │   ├── passwords
        │   │   ├── email.blade.php
        │   │   └── reset.blade.php
        │   └── register.blade.php
        ├── layouts
        │   ├── app.blade.php
        │   ├── footer.blade.php
        │   ├── navbars
        │   │   ├── auth.blade.php
        │   │   └── navs
        │   │       ├── auth.blade.php
        │   │       └── guest.blade.php
        │   └── page_templates
        │       ├── auth.blade.php
        │       └── guest.blade.php
        ├── pages
        │   ├── dashboard.blade.php
        │   ├── icons.blade.php
        │   ├── map.blade.php
        │   ├── notifications.blade.php
        │   ├── tables.blade.php
        │   ├── typography.blade.php
        │   └── upgrade.blade.php
        ├── profile
        │   └── edit.blade.php
        ├── users
        │   ├── create.blade.php
        │   ├── edit.blade.php
        │   └── index.blade.php
        └── welcome.blade.php
```

## Browser Support

At present, we officially aim to support the last two versions of the following browsers:

<img src="https://github.com/creativetimofficial/public-assets/blob/master/logos/chrome-logo.png?raw=true" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/firefox-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/edge-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/safari-logo.png" width="64" height="64"> <img src="https://raw.githubusercontent.com/creativetimofficial/public-assets/master/logos/opera-logo.png" width="64" height="64">


## Resources
- Demo: <https://paper-dashboard-laravel.creative-tim.com/?ref=mdlp-readme>
- Download Page: <https://www.creative-tim.com/product/paper-dashboard-laravel?ref=mdl-readme>
- Documentation: <https://paper-dashboard-laravel.creative-tim.com/docs/getting-started/laravel-setup.html?ref=mdl-readme>
- License Agreement: <https://www.creative-tim.com/license>
- Support: <https://www.creative-tim.com/contact-us>
- Issues: [Github Issues Page](https://github.com/laravel-frontend-presets/paper-dashboard/issues)
- **Dashboards:**

| HTML | LARAVEL |
| --- | --- |
| [![Paper Dashboard 2 HTML](https://s3.amazonaws.com/creativetim_bucket/products/50/original/opt_md_thumbnail.jpg?1522232645)](https://demos.creative-tim.com/paper-dashboard/examples/dashboard.html?ref=mdl-readme) | [![Paper Dashboard 2 Laravel](https://s3.amazonaws.com/creativetim_bucket/products/154/original/opt_md_laravel_thumbnail.jpg?1554814177)](https://paper-dashboard-laravel.creative-tim.com/?ref=mdl-readme)

## Change log

Please see the [changelog](CHANGELOG.md) for more information on what has changed recently.

## Credits

- [Creative Tim](https://creative-tim.com/?ref=mdl-readme)
- [UPDIVISION](https://updivision.com)

## Reporting Issues

We use GitHub Issues as the official bug tracker for the Paper Dashboard 2 Laravel. Here are some advices for our users that want to report an issue:

1. Make sure that you are using the latest version of the Paper Dashboard 2 Laravel. Check the CHANGELOG from your dashboard on our [website](https://www.creative-tim.com/?ref=mdl-readme).
2. Providing us reproducible steps for the issue will shorten the time it takes for it to be fixed.
3. Some issues may be browser specific, so specifying in what browser you encountered the issue might help.

## Licensing

- Copyright 2019 Creative Tim (https://www.creative-tim.com/?ref=mdl-readme)
- [Creative Tim License](https://www.creative-tim.com/license).


## Useful Links

- [Tutorials](https://www.youtube.com/channel/UCVyTG4sCw-rOvB9oHkzZD1w)
- [Affiliate Program](https://www.creative-tim.com/affiliates/new) (earn money)
- [Blog Creative Tim](http://blog.creative-tim.com/)
- [Free Products](https://www.creative-tim.com/bootstrap-themes/free) from Creative Tim
- [Premium Products](https://www.creative-tim.com/bootstrap-themes/premium?ref=mdl-readme) from Creative Tim
- [React Products](https://www.creative-tim.com/bootstrap-themes/react-themes?ref=mdl-readme) from Creative Tim
- [Angular Products](https://www.creative-tim.com/bootstrap-themes/angular-themes?ref=mdl-readme) from Creative Tim
- [VueJS Products](https://www.creative-tim.com/bootstrap-themes/vuejs-themes?ref=mdl-readme) from Creative Tim
- [More products](https://www.creative-tim.com/bootstrap-themes?ref=mdl-readme) from Creative Tim
- Check our Bundles [here](https://www.creative-tim.com/bundles??ref=mdl-readme)

### Social Media

### Creative Tim:

Twitter: <https://twitter.com/CreativeTim?ref=mdl-readme>

Facebook: <https://www.facebook.com/CreativeTim?ref=mdl-readme>

Dribbble: <https://dribbble.com/creativetim?ref=mdl-readme>

Instagram: <https://www.instagram.com/CreativeTimOfficial?ref=mdl-readme>

### Updivision:

Twitter: <https://twitter.com/updivision?ref=mdl-readme>

Facebook: <https://www.facebook.com/updivision?ref=mdl-readme>

Linkedin: <https://www.linkedin.com/company/updivision?ref=mdl-readme>

Updivision Blog: <https://updivision.com/blog/?ref=mdl-readme>

## Credits

- [Creative Tim](https://creative-tim.com/?ref=mdl-readme)
- [UPDIVISION](https://updivision.com)
