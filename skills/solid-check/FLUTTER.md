# Flutter / Dart Yardstick

The Dart-side check points for [`solid-check`](SKILL.md), applied on top of the questions in `SKILL.md` step 2.

## SRP
- Size smells: a `build()` over 100 lines, a `State` class over 300 lines, or a Bloc / Notifier holding more than 6 unrelated methods.
- Does the widget fetch, parse, and lay out all at once? Repository, use case, and widget are three layers.
- Did a private `_buildHeader()` helper returning a Widget appear? That is a widget class asking to be born.

## OCP
- Extension points in Dart: an abstract class, a `sealed class` + `switch`, or a builder callback passed in.
- Does a new case mean editing an old `switch` on an enum or a type string, rather than adding a subtype?

## LSP
- Read every `@override` in scope: does it narrow a parameter type, widen a return type, or throw where the parent declares none?

## ISP
- The tell for a forced method: an empty body, or `throw UnimplementedError()`.
- Does a widget take a wide config object when it reads two fields? Pass the two.

## DIP
- Does a widget or use case build its own `Dio`, `http.Client`, `SharedPreferences`, `FirebaseFirestore`, or concrete repository, with nothing injected through the constructor, `ref`, or the locator?
- Is the outside I/O (network, disk, platform channel, clock, random) behind an abstract layer a test can swap out?
- Does business logic import `package:flutter/material.dart`? A layer that knows widgets cannot be tested headless.

## IoC / container use
- Which container did the project pick — `get_it` / `injectable`, Riverpod, `provider`, or plain constructor passing? Judge by that one, and check it is the only one in play.
- Is there a service location anti-pattern — a run-time `GetIt.I<SomeService>()` deep inside a widget, in place of a constructor parameter or a provider read?
- Does each abstract type have a matching registration, and do the registrations sit in one wiring place rather than scattered?
- Does the lifetime fit the use — `registerLazySingleton` vs `registerFactory` — and does per-user or per-screen state hide inside a singleton?
- Riverpod: does a provider hold the dependency, so a test can `overrideWith` it?

## Extra checks
- Do `StreamSubscription`, `AnimationController`, `TextEditingController`, and timers each pair with a `dispose()`?
- Is a `BuildContext` used after an `await` without a `mounted` guard?
- Does `setState` sit in a widget where a Bloc / Notifier / ValueNotifier already owns that state?
- Is state mutated in place where the tree expects a fresh immutable value (`copyWith`, `freezed`)?
- Is a `late` field or a `!` standing in for a type the design should have made non-nullable?
