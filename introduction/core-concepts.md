---
layout: page 
title: Core Concepts
nav_order: 2
---
# Core Concepts
{: .no_toc }
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Advanced Custom Fields

GeniePress was designed to work closely with [Advanced Customer Fields](https://www.advancedcustomfields.com/).

GeniePress removes the need to create and manage schema through the WordPress backend.
All ACF schema can be created and managed through code.

- Manage acf schema in code
- Create reusable sets of fields
- Seamlessly push changes into production
- Attach schema to custom posts types providing a fluent interface for fields.

## Separation of concerns 
GeniePress comes bundled with twig, and this allows you to never have to mix html and php again. 

See [Views](/reference/views)

## Components
Genie expects you to divide up your code into self-contained classes called components.

Each class should implement the `GenieComponent` interface

Each component should implement a public static method called `setup` where 
Custom Posts are registered, hooks and filters defined etc

```php
<?php

namespace Theme;
use GeniePress\Interfaces\GenieComponent;

class MyComponent implements GenieComponent {
    public static function setup() {
        // Add hooks / post types / api calls etc  
    }
}
```

Each component has to be added to the `withComponents()` method when you start 
Genie (either from functions.php of your theme or from the main plugin file).

```php
<?php

 /**
 * Wordpress Plugin header  
 */

namespace Theme;
use GeniePress\Genie;

Genie::createPlugin()
    ->withComponents([
        MyComponent::Class
    ])
    ->start();
```

GeniePress will run the `setup()` method of your class during startup.
