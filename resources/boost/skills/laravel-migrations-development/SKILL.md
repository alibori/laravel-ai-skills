---
name: laravel-migrations-development
description: 
  This skill provides guidance on developing and managing Laravel migrations, including creating and modifying database schema.
  Activate this skill when working with Laravel migrations.
---

# Laravel Migrations Development

## When to use this skill
Use this skill when working with Laravel migrations, such as when creating new database tables, modifying existing tables, or managing database schema changes in a Laravel application.

## Key Rules

- **NO down method**: Do not include a `down` method in your migration files. This skill focuses on the push forward approach to database migrations, and the `down` method is not necessary.
- **NO `enum` type**: Avoid using the `enum` type in your migrations. Instead, use string or integer types to represent enumerated values.
- **NO default values**: Do not set default values for columns in your migrations. Default values can lead to unexpected behavior and should be handled at the application level instead.
- **Specify column lengths**: Always specify the length for string columns (e.g., `string('name', 255)`) to ensure consistency and avoid potential issues with database engines that have different default lengths. Set a reasonable length based on the expected data for that column.

### `after` helper vs `after` callback

When adding a new column to an existing table, you can use the `after` helper or the `after` callback to specify the position of the new column.

In case you are adding only one column, you can use the `after` helper:

```php
$table->string('new_column', 100)->after('existing_column');
```

In case you are adding multiple columns in order after another existing one you must use the `after` callback:

```php
$table->after('existing_column', function($table) {
    $table->string('new_column_1', 100);
    $table->string('new_column_2', 50);
    // ...
});
```

