---
name: change-database-to-default-rule
description: Change table and column names to snake_case. Use it when the user says "change to snake case", "make it snake_case", "database naming rule", or needs to change table columns to the Laravel standard names.
---

# Change a Table to snake case

## Need
 - Read the $ARGUMENTS class, and change its database columns to the snake_case naming rule.
 - Change every reference to the $ARGUMENTS class so it fits the new naming rule.

## What It Does

### Make a migration
- The database name should change to snake_case, and to the plural form.
- The column names should change to snake_case.

### Change the $table property of the $ARGUMENTS class:
- The database name should change to snake_case, and to the plural form.

### Change the column names of the $ARGUMENTS class:
- The column names should change to snake_case.

### Check every file for references to the $ARGUMENTS class
- You should update every spot where this Eloquent column name is used by other classes. Note that the column may also be used in other Eloquent relationships.
- Keep in mind the check is not limited to these folders:
 - app/
 - resources/
 - tests/

## Notes
- When you write code, pay special heed to how easy it is to test, how easy it is to read, and how low the coupling is.
- When it fits, try to use PHP Attribute and Laravel Attribute in place of the old way.
