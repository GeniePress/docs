---
layout: page 
title: Shortcodes
nav_order: 20
---

# Shortcodes

{: .no_toc }
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Main Usage

You can use the `AddShortcode` utility to hook create shortcodes.

## Basic usage

```php
use GeniePress\Utilities\AddShortcode;

AddShortcode::called('test_me')
  ->run(function($attributes,$content) {
    // process your shortcode here
});
```

## Using Views with Shortcodes

```php
use GeniePress\Utilities\AddShortcode;
use GeniePress\View;

AddShortcode::called('user')
  ->run(function($attributes,$content) {
     
    return View::with('shortcodes/user.twig')
      ->addVars([
         'user' => wp_get_current_user(),
         'attributes' => $attributes,
      ])
      ->render();
});
```

The shortcode could be user :

```html
<p>Hello [user field=name],</p>
```

and the `shortcodes/user.twig` file

```twig
{% raw %}
{% if attributes.field == 'name' and user.first_name %}
  {{ user.first_name }}
{% else %}
  there
{% endif %}
{% endraw %}
```

Learn more about using [Views](../views/)
