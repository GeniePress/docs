---
layout: page 
title: Ajax Handling
parent: Reference
---

# Ajax Handling
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
GeniePress provides a convenient way to handle ajax calls, including nonces.

## Enabling Ajax Handling

When you initiate Genie, be sure to use the `enableAjaxHandler()` options

```php
use GeniePress\Genie;
use Theme\MyComponent;

Genie::createPlugin()
  ->enableAjaxHandler()
  ->withComponents([
    MyComponent::class
  ])
  ->start();
```

In our component's `setup()` method we can register an ajax endpoint 

```php
<?php

namespace Theme;

use GeniePress\Utilities\RegisterAjax;

class MyComponent {

    public static function setup() { 
    
        RegisterAjax::url('testimonial/create')
            ->run(function ($name, $testimonial) {
                // Save the testimonial
                return [
                  'success' => true
                ];       
            });
    }
}
```

GeniePress uses reflection to determine what parameters are required in your callback (can be a closure, method or function).

## Accessing the full endpoint url
In our template, we can refer to the ajax endpoint a number of ways

in php:

```php
use GeniePress\AjaxHandler;
$endpoint = AjaxHandler::generateUrl('testimonial/create');
```

or in twig:

{% raw %}
```html
<script>
  var endpoint = '{{ ajax_url('testimonial/create') }}';
</script>
```

or passing it to a Vue Component

```html
<div>
  <vue-component endpoint="{{ ajax_url('testimonial/create') }}" />
</div> 
```
{% endraw %}

## Sending a request to the endpoint

Here's an example of sending data to the endpoint from jQuery in a WordPress template
{% raw %}
```javascript
<script>
    jQuery(document).ready(function ($) {
        var buttonClicked = false;
        $('#contactForm').submit(function (event) {
            event.preventDefault();
            if (buttonClicked) {
                return;
            }
            buttonClicked = true;
            $.ajax({
                url: '{{ ajax_url('testimonial/create') }}',
                data: JSON.stringify({
                    name: $('#name').val(),
                    testimonial: $('#testimonial').val()
                }),
                type: 'POST',
                dataType: 'json',
                contentType: 'application/json',
                complete: function (data) {
                    buttonClicked = false
                    if (data.status === 200) {
                      // Success !
                    } else { 
                      // Show an error  
                    }       
                }
            });
        })
    });
</script>
```

**Note:** All ajax calls use the POST method. In future versions of GeniePress we will allow different verbs.


{% endraw %}

## Throwing errors / Failing
Throwing an exception from your callback function will be picked up by GeniePress

```php
use GeniePress\Exceptions\GenieException;
use GeniePress\Utilities\RegisterAjax;
RegisterAjax::url('testimonial/create')
    ->run(function ($name, $testimonial) {
        if ($name === 'sunil') { 
           Throw GenieException::withMessage('Sorry Sunil cannot complete forms')
             ->withCode(1001)
             ->withData(compact('name','testimonial'));
        }
        
        return [
          'success' => true
        ];       
    });
```
Your ajax call will need to handle a 400 error.  The data sent on the exception will be passed in the response.
