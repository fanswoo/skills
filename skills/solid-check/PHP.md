# PHP / Laravel Yardstick

The PHP-side check points for [`solid-check`](SKILL.md), applied on top of the questions in `SKILL.md` step 2.

## SRP
- Size smells: a class over 300 lines, or with more than 8 public methods.
- Does an Eloquent Model hold query or service logic that is not its job?
- Has the controller become a "fat controller"? It belongs in an Action / Service / Form Request.

## OCP
- Extension points in PHP: an interface, an abstract class, or a Strategy / Factory.

## ISP
- The tell for a forced method: an empty body, or `throw new BadMethodCallException`.

## DIP
- Is there a hidden dependency through a facade, `app()`, `resolve()`, or `static::`?

## IoC / container use
- Is the service registered through a Service Provider? If it needs to be a singleton, does it use `singleton`?
- Is there a service location anti-pattern — a run-time `app(SomeService::class)` in place of constructor inject?
- Does the interface have a matching binding, and does that binding live in a sensible Provider?
- Are contextual binding and tagged services used well?

## Extra checks
- Should a PHP Attribute or a Laravel Attribute replace the old way?
- Is there a static method that holds state, a global variable, or a Singleton anti-pattern?
