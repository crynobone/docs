---
title: Asset Component

---

[![Latest Stable Version](https://img.shields.io/github/release/orchestral/asset.svg?style=flat)](https://packagist.org/packages/orchestra/asset)
[![Total Downloads](https://img.shields.io/packagist/dt/orchestra/asset.svg?style=flat)](https://packagist.org/packages/orchestra/asset)
[![MIT License](https://img.shields.io/packagist/l/orchestra/asset.svg?style=flat)](https://packagist.org/packages/orchestra/asset)

Asset Component is a port of Laravel 3 Asset for Orchestra Platform. The component main functionality is to allow asset declaration to be handle dynamically and asset dependencies can be resolve directly from the container. It however is not intended to becoma an asset pipeline package for Laravel, for such purpose we would recommend to use Grunt or Gulp.

## Table of Content {#toc}

* [Version Compatibility](#compatibility)
* [Installation](#installation)
* [Configuration](#configuration)
* [Usage](#usage)
  - [Registering Assets](#registering-assets)
  - [Dumping Assets](#dumping-assets)
  - [Asset Dependencies](#asset-dependencies)
  - [Asset Containers](#asset-containers)
  - [Asset Versioning](#asset-versioning)
* [Change Log]({doc-url}/components/asset/changes#v3-0)
* [Resources](#resources)

## Version Compatibility {#compatibility}

Laravel    | Asset
:----------|:----------
 4.0.x     | 2.0.x
 4.1.x     | 2.1.x
 4.2.x     | 2.2.x
 5.0.x     | 3.0.x

## Installation {#installation}

To install through composer, simply put the following in your `composer.json` file:

```json
{
	"require": {
		"orchestra/asset": "3.0.*"
	}
}
```

And then run `composer install` from the terminal.

### Quick Installation {#quick-installation}

Above installation can also be simplify by using the following command:

```bash
composer require "orchestra/asset=3.0.*"
```

## Configuration {#configuration}

Next add the service provider in `config/app.php`.

```php
'providers' => [

	// ...

	'Orchestra\Asset\AssetServiceProvider',
],
```

### Aliases

You might want to add `Orchestra\Support\Facades\Asset` to class aliases in `config/app.php`:

```php
'aliases' => [

	// ...

	'Asset' => 'Orchestra\Support\Facades\Asset',
],
```

## Usage {#usage}

### Registering Assets {#registering-assets}

The Asset class provides a simple way to manage the CSS and JavaScript used by your application. To register an asset just call the add method on the Asset class:

#### Registering an asset:

```php
Asset::add('jquery', 'js/jquery.js');
```

The add method accepts three parameters. The first is the name of the asset, the second is the path to the asset relative to the public directory, and the third is a list of asset dependencies (more on that later). Notice that we did not tell the method if we were registering JavaScript or CSS. The add method will use the file extension to determine the type of file we are registering.

### Dumping Assets {#dumping-assets}

When you are ready to place the links to the registered assets on your view, you may use the styles or scripts methods:

Dumping assets into a view:

```html
<head>
	{!! Asset::styles() !!}
	{!! Asset::scripts() !!}
</head>
```

Above code can also be simplified as:

```html
<head>
	{!! Asset::show() !!}
</head>
```

### Asset Dependencies {#asset-dependencies}

Sometimes you may need to specify that an asset has dependencies. This means that the asset requires other assets to be declared in your view before it can be declared. Managing asset dependencies couldn't be easier in Laravel. Remember the "names" you gave to your assets? You can pass them as the third parameter to the add method to declare dependencies:

Registering a bundle that has dependencies:

```php
Asset::add('jquery-ui', 'js/jquery-ui.js', 'jquery');
```

In this example, we are registering the jquery-ui asset, as well as specifying that it is dependent on the jquery asset. Now, when you place the asset links on your views, the jQuery asset will always be declared before the jQuery UI asset. Need to declare more than one dependency? No problem:

Registering an asset that has multiple dependencies:

```php
Asset::add('jquery-ui', 'js/jquery-ui.js', ['first', 'second']);
```

### Asset Containers {#asset-containers}

To increase response time, it is common to place JavaScript at the bottom of HTML documents. But, what if you also need to place some assets in the head of your document? No problem. The asset class provides a simple way to manage asset containers. Simply call the container method on the Asset class and mention the container name. Once you have a container instance, you are free to add any assets you wish to the container using the same syntax you are used to:

Retrieving an instance of an asset container:

```php
Asset::container('footer')->add('example', 'js/example.js');
```

Dumping that assets from a given container:

```php
{!! Asset::container('footer')->scripts() !!}
```

### Asset Versioning {#asset-versioning}

Another option to increase response time is by utilizing browser caching, while there few ways to do this we pick last modified time as our way to version the Asset.

```php
Asset::container()->addVersioning();

// or alternatively
Asset::addVersioning();
```

> Note: this would only work with local asset.

You can remove adding versioning number by using:

```php
Asset::container()->removeVersioning();

// or alternatively
Asset::removeVersioning();
```

## Resources

* [Github](https://github.com/orchestral/asset)
* [Packagist](https://packagist.org/packagist/orchestra/asset)
