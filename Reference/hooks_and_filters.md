---
layout: page 
title: Hooks & Filters 
parent: Reference
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

You can use the `HookInto` utility to hook into actions and filters. Genie uses
Reflection to work out how many parameters are being use in the closure or
function

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
## Specifying Parameter count

There's no need to do this as GeniePress uses reflection to count the 
parameters in the callback or closure. 

## Using a callback

any callable is accepted by the `run()` method.

```php
use GeniePress\Utilities\HookInto;
HookInto::action('init', 20)
  ->run( [\GeniePress\Debug::class, 'dd']);
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

rather than use action - you can use the `filter()` method:

```php
use GeniePress\Utilities\HookInto;
HookInto::filter('the_content')
    ->run( function($content) { 
      // run something
      return $content;
  });
```
