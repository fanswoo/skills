---
name: change-eloquent-to-default-rule
description: Change FF\ORM\ORM classes to the standard Laravel Eloquent. Use it when the user says "change to Eloquent", "migrate to Eloquent", "use Laravel ORM", or needs to move a custom ORM to standard Eloquent.
---

# Change to Eloquent

## Need
- Read the $ARGUMENTS class, and change it to the Laravel standard use rules.
- Change every reference to the $ARGUMENTS class so it fits the new naming rule.

## What It Does

### Change the parent class of the $ARGUMENTS class:
 - `FF\ORM\ORM` should change to `Illuminate\Database\Eloquent\Model`.
 - If it uses the `SoftDeletes` feature, change it to the `Illuminate\Database\Eloquent\SoftDeletes` trait, and make sure the `deleted_at` column is changed.
 - If it uses the `Timestamps` feature, make sure the `created_at` and `updated_at` columns exist, and remove any custom timestamp logic.
 - Watch the Attributes. In the old FF\ORM\ORM the Mutators are named xxxGet() xxxSet(); in Eloquent they are "xxx(): Attribute".

### Check every file for references to the $ARGUMENTS class
You should use the Laravel standard Eloquent methods. Keep in mind the check is not limited to these folders:
- app/
- resources/
- tests/

## Notes
- When you write code, pay special heed to how easy it is to test, how easy it is to read, and how low the coupling is.
- When it fits, try to use PHP Attribute and Laravel Attribute in place of the old way.
- The path of the `FF\ORM\ORM` class is at '\ligo\vendor\fanswoo\framework-core\src\ORM\ORM.php'.
- You should use the mcp to look up Laravel v11, to make sure it fits the standard.
