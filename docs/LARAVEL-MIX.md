- [Basic Usage](INDEX.md)
- [Structs](STRUCTS.md)
- [Hooks](HOOKS.md)
- **[Laravel-Mix](LARAVEL-MIX.md)**
- [Example App](EXAMPLE-APP.md)

## Laravel-Mix Integration

You can include your `mix-manifest.json` file generated by [Laravel-Mix](https://laravel-mix.com) to automatically add preload/prefetch link elements to your document head.

### Basic example

```php
seo()
    ->mix()
    ->load();
```

### Extended usage

By default, all assets found in your mix file are prefteched. You can read more about preloading and prefetching [in this article by css-tricks.com](https://css-tricks.com/prefetching-preloading-prebrowsing/).

```php
seo()
    ->mix()
    ->rel('preload')
    ->load(public_path('custom-manifest.json'));
```

### Filter assets

By default, all assets are added to the document head. You can specify filters or rejections to hide certain assets like admin scripts. The callbacks are passed through the Laravel collection instance.

```php
seo()
    ->mix()
    ->reject(function($path, $url) {
        return strpos($path, 'admin') !== false;
    })
    ->filter(function($path, $url) {
        return strpos($path, '.js') !== false;
    })
    ->load();
```

### Example

```php
seo()
    ->mix()
    ->load();
```

**mix-manifest.json**

```json
{
  "/js/app.js": "/js/app.js?id=4c8b94c7a94dd6137b79",
  "/css/app.css": "/css/app.css?id=35f9f53a2e3a7804169d"
}
```

**document `<head>`**

```html
<link rel="prefetch" href="http://localhost/js/app.js?id=4c8b94c7a94dd6137b79" />
<link rel="prefetch" href="http://localhost/css/app.css?id=35f9f53a2e3a7804169d" />
```