---
name: laravel-models-development
description: 
  This skill provides guidance on developing and managing Laravel models, including related features in other files like AppServiceProvider, factories, and seeders.
  Activate this skill when working with Laravel models or related features in other files like AppServiceProvider, factories, and seeders.
---

# Laravel Models Development

## When to use this skill
Use this skill when working with Laravel models or related features in other files like AppServiceProvider, factories, and seeders.

## Key Rules

- **NO fillable property**: Do not use the `$fillable` property in your models. What you write in your models is responsibility of your business logic, and the `$fillable` property is not necessary.
- **NO guarded property**: Do not use the `$guarded` property in your models. What you write in your models is responsibility of your business logic, and the `$guarded` property is not necessary.

## Definitions in the AppServiceProvider

Always define models strict and unguarded in the `AppServiceProvider` to ensure that mass assignment is handled as I want and to avoid N+1 and other problems during development. This is done by adding the following code in the `boot` method of the `AppServiceProvider`:

```php
use Illuminate\Database\Eloquent\Model;

public function boot(): void
{
    $this->configureModels();
}

private function configureModels(): void
{
    Model::unguard();

    Model::shouldBeStrict( ! app()->isProduction());
}
```

