---
layout: page 
title: Views
nav_order: 20
---

# Genie Views
{: .no_toc }
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Genie uses Twig

Genie uses twig behind the scenes to ensure separation of concerns. By default,
Genie looks for views in the `src/twig` folder.

## Example Component using a View

```php
<?php

namespace GeniePluginExample;

use GeniePress\Interfaces\GenieComponent;
use GeniePress\Utilities\HookInto;
use GeniePress\View;

class Footer implements GenieComponent
{

    public static function setup()
    {
       HookInto::action('wp_footer')
         ->run(function() {
             View::with('footer.twig')
              ->display();
       });
    }
}
```

Genie will look into the `src/twig` folder for `footer.twig`

`footer.twig`

```html
<p>This is a twig file!</p>
```

## Passing Variables to templates

### Individual variables

```php
use GeniePress\View;

View::with('footer.twig')
  ->addVar('user', get_current_user())
  ->display();
```

### As an array

```php
use GeniePress\View;

View::with('footer.twig')
->addVars([
  'time' => time(),
  'user' => get_current_user()
])
->display();
```

## Returning html

use `render()` to return html

```php
use GeniePress\View;

$html = View::with('footer.twig')
->addVars([
  'name' => 'sunil',
  'user' => get_current_user()
])
->render();
```

## Changing the default path for views

When creating a genie plugin or theme you can specify the location of twig files
using the `useViewsFrom()` method.

```php
use GeniePress\Genie;

Genie::createPlugin()
    ->withComponents([
    //component classes
    ])
    ->useViewsFrom( 'my_dir/my_folder')
    ->start();
```
