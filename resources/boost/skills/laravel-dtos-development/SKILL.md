---
name: laravel-dtos-development
description: 
  This skill provides guidance on developing and managing Data Transfer Objects (DTO) in a Laravel application.
  Activate this skill when working with DTOs in Laravel applications. DTOs are used to encapsulate data and transfer it between different layers of the application, such as from controllers to services or action classes.
---

# Laravel DTOs Development

## When to use this skill

Use this skill when working with Data Transfer Objects (DTOs) in a Laravel application. DTOs are used to encapsulate data and transfer it between different layers of the application, such as from controllers to services or action classes.

## Overview

Data Transfer Objects (DTOs) are simple, immutable objects that encapsulate data and carry it between the layers of an application — for example, from controllers to action or service classes. They provide a clear, statically analysable contract, replacing loose associative arrays with typed properties.

## Key Rules

- **Immutable**: Declare the DTO `final` and its properties `readonly` so data cannot change after construction.
- **Typed properties**: Every property has an explicit type. Use enums and nullable types where appropriate instead of raw strings.
- **No behaviour**: A DTO carries data; it does not contain business logic, persistence, or validation. Validation happens upstream in a `FormRequest`.

## Methods

When creating DTOs, always define these methods:

- **`public static function fromRequest(Request $request): self`**: Builds a DTO instance from an incoming HTTP request, mapping request data to typed properties.
- **`public function toArray(): array`**: Converts the DTO into an associative array, useful for passing data to actions or persistence that expect an array.

## Example DTO Implementation

```php
namespace App\DTOs;

use Illuminate\Http\Request;

final class UserDTO
{
    public function __construct(
        public readonly string $name,
        public readonly string $email,
        public readonly int $age
    ) {}

    public static function fromRequest(Request $request): self
    {
        return new self(
            name: $request->string('name')->value(),
            email: $request->string('email')->value(),
            age: $request->integer('age')
        );
    }

    public function toArray(): array
    {
        return [
            'name' => $this->name,
            'email' => $this->email,
            'age' => $this->age,
        ];
    }
}
```

## Related skills

- DTOs are typically built in controllers (`laravel-controllers-development`) and consumed by action classes (`laravel-actions-development`).
