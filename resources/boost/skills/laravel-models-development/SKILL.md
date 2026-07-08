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

- **NO `$fillable` property**: Do not use the `$fillable` property in your models. Deciding what data is written is the responsibility of your business logic (DTOs, actions), not the model.
- **NO `$guarded` property**: Do not use the `$guarded` property in your models. Mass assignment protection is handled globally by unguarding models in the `AppServiceProvider` (see below).
- **Casts in the `casts()` method**: Define casts in the `casts()` method, not the `$casts` property. This is the modern Laravel API and allows using enums, `AsArrayObject`, and other class-based casts cleanly.
- **Type every relationship**: Give every relationship method an explicit return type (`HasMany`, `BelongsTo`, …) for static analysis and IDE support.

## Definitions in the AppServiceProvider

Always define models strict and unguarded in the `AppServiceProvider`. Unguarding enables mass assignment (safe because input is shaped by DTOs and validated by `FormRequest` classes upstream), and strict mode surfaces N+1 queries, accessing missing attributes, and silently discarded attributes during development. Add this to the `boot` method:

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

`shouldBeStrict` is disabled in production so a lazy-loading or missing-attribute violation cannot take down a live request.

## Example Model

```php
namespace App\Models;

use App\Enums\UserStatus;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

final class User extends Model
{
    protected function casts(): array
    {
        return [
            'status' => UserStatus::class,
            'email_verified_at' => 'datetime',
            'settings' => 'array',
        ];
    }
    
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }
}
```

## Factories and Seeders

- Keep factory definitions aligned with the model's real attributes and casts; return enum instances or arrays that match the declared casts.
- Use states for meaningful variations (e.g. `unverified()`) instead of overriding attributes ad hoc at every call site.

## Final Note

If the project uses the `barryvdh/laravel-ide-helper` package, run `php artisan ide-helper:models -M` after creating or modifying models to regenerate model helper files for better IDE support and autocompletion.
