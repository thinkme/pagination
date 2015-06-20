## Overview

if use laravel4.0/laravel4.1/laravel4.2 click this:
https://packagist.org/packages/desmart/pagination

This package is an extension for Laravel5 pagination module.
Modified from desmart / pagination Thanks radmen https://github.com/DeSmart/pagination

It provides new functionalities:

* route based url generator
* helpers for template render

## Installation

To `composer.json` add: `"thinkme/pagination": "dev-master"` and then run `composer update thinkme/pagination`.

## Compatibilty

This package should not break compatibility with Laravel pagination module.

### Laravel 5.0

## Method overview

### General usage
* `withQuery()` - bind query parameters to url generator (by default query parameters are included). Works only for url generating from routes.
* `withoutQuery()` - don't bind query parameters
* `route($route[, array $parameters])` - use given route for generating url to pages (it can be route name, or instance of `Illuminate\Routing\Route`)

### For templates
* `pagesProximity($proximity)` - set pages proximity
* `getPagesRange()` - get list of pages to show in template (includes proximity)
* `canShowFirstPage()` - check if can show first page (returns `TRUE` when first page is not in list generated by `getPagesRange()`)
* `canShowLastPage()` - check if can show last page (returns `TRUE` when last page is not in list generated by `getPagesRange()`)

## Example usage

### In controller

```php
// example route (routes.php)
Route::get('list-{page}.html', ['as' => 'list.page', 'uses' => 'PhotoController@index']);

// use the current route
$list = new Paginator();
$list = list->make($item, $count, 1, $page, [
            'path' => Paginator::resolveCurrentPath(),
        ]);
$list->route('list.page');
$list->pagesProximity(3);

// IF use yourself Presenter Class 
Paginator::presenter(function() use ($list) {
            return new MyPresenter($list);
        });
```

### In view
```php
// list.blade.php

@foreach ($list as $item)
{{-- show item --}}
@endforeach

{!! $list->links('paginator') !!}

// if use yourself Presenter Class

{!! $list->render() !!}

// paginator.blade.php

@if ($paginator->lastPage() > 1)
  @foreach ($paginator->getPagesRange() as $page)
    {{ $page }}
  @endforeach
@endif

// or this

@if ($paginator->lastPage() > 1)
    <div class="pagination">
        @if($paginator->currentPage()>1)
            <a href="{{$paginator->url($paginator->currentPage()-1)}}" class="first"></a>
        @else
            <a href="javascript:;" class="first-none"></a>
        @endif
            @foreach ($paginator->getPagesRange() as $page)
                @if($paginator->currentPage()==$page)
                    <span class="current">{{$page}}</span>
                @else
                    <a href="{{$paginator->url($page)}}">{{$page}}</a>
                @endif
            @endforeach
            @if($paginator->currentPage()<$paginator->lastPage())
                <a href="{{$paginator->url($paginator->currentPage()+1)}}" class="last last-none"></a>
            @else
                <a href="javascript:;" class="last-none"></a>
            @endif
    </div>
@endif
```

## License

This package is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
