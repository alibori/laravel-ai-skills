---
name: laravel-controllers-development
description: 
  This skill provides guidance on developing and managing Laravel controllers.
  Activate this skill when working with Laravel controllers or related features such as routes.
---

# Laravel Controllers Development

## When to use this skill

Use this skill when working with Laravel controllers or related features such as routes.

## Key Rules

- **NO validation**: Do not add validation logic in the controller. Validation should be handled in dedicated `FormRequest` classes.
- **NO business logic**: Do not add business logic in the controller. Business logic should be handled in dedicated action/service/use case classes.
- **NO error handling**: Do not add error handling logic in the controller. Error handling should be handled in `bootstrap/app.php` through dedicated exception classes or middlewares.
- **Type everything**: Type-hint the `FormRequest` and any action classes in the method signature so they resolve through the container, and declare an explicit return type.

## Overview

Controllers in Laravel handle incoming HTTP requests and return responses. They bridge the application's routes and the business logic, and should stay lean by delegating rather than implementing logic themselves.

They are responsible for:

- Receiving requests.
- Delegating tasks to appropriate action or service classes.
- Receiving results from those classes.
- Returning responses to the client.

## Flow

A controller method wires the layers together and does nothing else:

1. Type-hint a `FormRequest` — validation runs before the method body executes.
2. Build a DTO from the validated request.
3. Hand the DTO to an action class that performs the work.
4. Return a response built from the action's result.

## Example Controller

```php
namespace App\Http\Controllers;

use App\Actions\User\CreateUserAction;
use App\DTOs\UserDTO;
use App\Http\Requests\StoreUserRequest;
use Illuminate\Http\RedirectResponse;

final class UserController extends Controller
{
    public function __construct(
        private readonly CreateUserAction $createUserAction
    )
    {}

    public function store(StoreUserRequest $request): RedirectResponse
    {
        $user = $this->createUserAction->handle(UserDTO::fromRequest($request));

        return to_route('users.show', $user);
    }
}
```

## DTOs

For better static analysis and type safety, pass data from the controller to action/service classes as Data Transfer Objects (DTOs) rather than raw request arrays. DTOs define a clear contract for the data being handled, making the code easier to maintain and understand.

## Related skills

- Validation lives in `FormRequest` classes.
- Business logic lives in action classes (`laravel-actions-development`).
- Data passed to actions is shaped as a DTO (`laravel-dtos-development`).
