---
layout: page 
title: API Handling 
nav_order: 20
---

# API Handling

{: .no_toc }
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Introduction

GeniePress provides a simple way to provide api endpoints

## Enabling Api Handling

When you initiate Genie, be sure to use the `enableApiHandler()` option

```php
use GeniePress\Genie;
use Theme\MyComponent;

Genie::createPlugin()
  ->enableApiHandler()
  ->withComponents([
    MyComponent::class
  ])
  ->start();
```

Internally Genie uses a custom route to handle api requests as if they were ajax requests.

The `enableApiHandler()` takes two arguments

`$path = 'api` - the prefix of the api endpoint name
`$actionName = 'genie_api` = the internal action name used to route the request

**Note** If you enable the API handler, be sure to generate permalinks.

In our component's `setup()` method we can register an api endpoint

```php
<?php

namespace Theme;

use GeniePress\Utilities\RegisterApi;

class MyComponent {

    public static function setup() { 
    
        RegisterApi::get('posts')
            ->run(function () {
            
                $posts = Posts::get([
                    'numberposts' => -1,
                ]);
            
                // get some data 
                return [
                  'success' => true,
                  'data' => $posts,
                ];       
            });
    }
}
```

## Verbs

You can use the different verbs when you register the Api endpoint,

```php
RegisterApi::get('posts')->run(function(){}); 
RegisterApi::post('posts/create')->run(function(){}); 
RegisterApi::delete('posts/delete')->run(function(){}); 
RegisterApi::put('posts/update')->run(function(){});  
```

When required, GeniePress uses reflection to determine what parameters are required in your callback (can be a closure, method or function).

## Accessing the full endpoint url

In our template, we can refer to the ajax endpoint a number of ways

in php:

```php
use GeniePress\ApiHandler;
$endpoint = ApiHandler::generateUrl('posts');
```

or in twig:

{% raw %}

```html

<script>
    var endpoint = '{{ api_url('
    posts
    ') }}';
</script>
```

or passing it to a Vue Component

```html

<div>
    <vue-component endpoint="{{ api_url('posts') }}"/>
</div> 
```

{% endraw %}

## Throwing errors / Failing

Throwing an exception from your callback function will be picked up by GeniePress and respond with a 400 error.

```php
use GeniePress\Exceptions\GenieException;
use GeniePress\Request;
use GeniePress\Utilities\RegisterApi;

RegisterApi::get('posts')
    ->run(function () {

        if (!Request::has('type')) {
            Throw GenieException::withMessage('No type specified');
        }
        // get data 
        $data = [];
        
        return [
          'success' => true,
          'data' => $data
        ];        
    });
```

