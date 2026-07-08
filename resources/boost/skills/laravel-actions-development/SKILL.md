---
name: laravel-actions-development
description: 
  This skill provides guidance on developing and managing action classes in a Laravel application.
  Activate this skill when working with action classes in Laravel applications. Actions encapsulate a single unit of business logic and are typically invoked from controllers, jobs, commands, or other action classes.
---

# Laravel Actions Development

## When to use this skill

Use this skill when working with action classes in a Laravel application. Actions encapsulate a single unit of business logic and are typically invoked from controllers, jobs, commands, or other action classes.

## Key Rules

- **Single responsibility**: Each action class does one thing. If an action grows multiple responsibilities, split it into smaller actions.
- **NO validation**: Do not add request validation in the action. Validation belongs in dedicated `FormRequest` classes before data reaches the action.
- **NO HTTP concerns**: Do not read from the request or build HTTP responses inside an action. Actions receive data (ideally a DTO) and return data or a model, never a `Response`.
- **Type safety**: Accept DTOs as input for a clear, statically analysable contract. Avoid passing loose associative arrays.

## Overview

Action classes hold the business logic that controllers, jobs, commands, and other actions delegate to. They keep the rest of the application lean by concentrating a single use case in one reusable, testable place.

An action is responsible for:

- Receiving typed input (a DTO or explicit typed arguments).
- Executing one unit of business logic.
- Returning a result (a model, DTO, primitive, or `void`).

## Structure

- Place actions in `App\Actions` (namespace them further by domain when it helps, e.g. `App\Actions\User`).
- Name the class after what it does, in verb form: `CreateUserAction`, `PublishPostAction`, `CancelSubscriptionAction`.
- Expose a single public method. Use `handle()` as the entry point so the action can also be resolved and called through the container.
- Declare the class `final` and inject dependencies through the constructor.

## Example Action Implementation

```php
namespace App\Actions\User;

use App\DTOs\UserDTO;
use App\Models\User;

final class CreateUserAction
{
    public function handle(UserDTO $data): User
    {
        return User::create($data->toArray());
    }
}
```

## Final Note

Actions should be easy to unit test in isolation: construct the class (or resolve it from the container), pass a DTO, and assert on the returned result without booting the HTTP layer.

## Related skills

- Actions are invoked from controllers (`laravel-controllers-development`).
- Input is passed as a DTO (`laravel-dtos-development`).
- Actions operate on models (`laravel-models-development`).
