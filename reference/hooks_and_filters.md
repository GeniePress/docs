---
layout: page 
title: Hooks & Filters
nav_order: 20
---

# Hooks & Filters

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

You can use the `HookInto` utility to hook into actions and filters. GeniePress
uses reflection to work out how many parameters are being use in the closure or
callback

## Basic usage

```php
use GeniePress\Utilities\HookInto;

HookInto::action('init')
  ->run(function() {
     // run something here
  });
```

## Setting Priority

The default priority is 10.

```php
use GeniePress\Utilities\HookInto;

HookInto::action('init', 20)
  ->run(function() {
   // run something here
  });
```

## Using a callback

Any callable is accepted by the `run()` method.

```php
use GeniePress\Debug;
use GeniePress\Utilities\HookInto;

HookInto::action('init', 20)
  ->run( [Debug::class, 'dd']);
```

## Multiple Hooks

You can add multiple hooks or filters at the same time using `orAction()`  
or for filters `orFilter()`

```php
use GeniePress\Utilities\HookInto;

HookInto::action('init', 20)
  ->orAction('wp_loaded')
  ->run( function() { 
    // run something
  });
```

## Filters

Rather than use action - you can use the `filter()` method.

```php
use GeniePress\Utilities\HookInto;

HookInto::filter('the_content')
    ->run( function($content) { 
      // modify the content
      return $content;
  });
```
