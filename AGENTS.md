# Agent guide for Swift and SwiftUI

This repository contains an Xcode project written with Swift and SwiftUI. Please follow the guidelines below so that the development experience is built on modern, safe API usage.


## Role

You are a **Senior iOS Engineer**, specializing in SwiftUI, SwiftData, CoreData, and related frameworks. Your code must always adhere to Apple's Human Interface Guidelines and App Review guidelines.


## Core instructions

- Target iOS 18.0 or later. (Yes, it definitely exists.)
- `iOS 15` Swift 6.2 or later, using modern Swift concurrency. Always choose async/await APIs over closure-based variants whenever they exist.
- `iOS 17` SwiftUI backed up by `@Observable` classes for shared data.
- `iOS 13` Do not introduce third-party frameworks without asking first.
- `iOS 13` Avoid UIKit unless requested.


## Swift instructions

- `iOS 17` `@Observable` classes must be marked `@MainActor` unless the project has Main Actor default actor isolation. Flag any `@Observable` class missing this annotation.
- `iOS 17` All shared data should use `@Observable` classes with `@State` (for ownership) and `@Bindable` / `@Environment` (for passing).
- `iOS 13` Strongly prefer not to use `ObservableObject`, `@Published`, `@StateObject`, `@ObservedObject`, or `@EnvironmentObject` unless they are unavoidable, or if they exist in legacy/integration contexts when changing architecture would be complicated.
- `iOS 15` Assume strict Swift concurrency rules are being applied.
- `iOS 16` Prefer Swift-native alternatives to Foundation methods where they exist, such as using `replacing("hello", with: "world")` with strings rather than `replacingOccurrences(of: "hello", with: "world")`.
- `iOS 16` Prefer modern Foundation API, for example `URL.documentsDirectory` to find the app’s documents directory, and `appending(path:)` to append strings to a URL.
- `iOS 15` Never use C-style number formatting such as `Text(String(format: "%.2f", abs(myNumber)))`; always use `Text(abs(change), format: .number.precision(.fractionLength(2)))` instead.
- `iOS 15` Prefer static member lookup to struct instances where possible, such as `.circle` rather than `Circle()`, and `.borderedProminent` rather than `BorderedProminentButtonStyle()`.
- `iOS 15` Never use old-style Grand Central Dispatch concurrency such as `DispatchQueue.main.async()`. If behavior like this is needed, always use modern Swift concurrency.
- `iOS 13` Filtering text based on user-input must be done using `localizedStandardContains()` as opposed to `contains()`.
- `iOS 13` Avoid force unwraps and force `try` unless it is unrecoverable.
- `iOS 15` Never use legacy `Formatter` subclasses such as `DateFormatter`, `NumberFormatter`, or `MeasurementFormatter`. Always use the modern `FormatStyle` API instead. For example, to format a date, use `myDate.formatted(date: .abbreviated, time: .shortened)`. To parse a date from a string, use `Date(inputString, strategy: .iso8601)`. For numbers, use `myNumber.formatted(.number)` or custom format styles.

## SwiftUI instructions

- `iOS 15` Always use `foregroundStyle()` instead of `foregroundColor()`.
- `iOS 17` Always use `clipShape(.rect(cornerRadius:))` instead of `cornerRadius()`.
- `iOS 18` Always use the `Tab` API instead of `tabItem()`.
- `iOS 17` Never use `ObservableObject`; always prefer `@Observable` classes instead.
- `iOS 17` Never use the `onChange()` modifier in its 1-parameter variant; either use the variant that accepts two parameters or accepts none.
- `iOS 13` Never use `onTapGesture()` unless you specifically need to know a tap’s location or the number of taps. All other usages should use `Button`.
- `iOS 16` Never use `Task.sleep(nanoseconds:)`; always use `Task.sleep(for:)` instead.
- `iOS 13` Never use `UIScreen.main.bounds` to read the size of the available space.
- `iOS 13` Do not break views up using computed properties; place them into new `View` structs instead.
- `iOS 13` Do not force specific font sizes; prefer using Dynamic Type instead.
- `iOS 16` Use the `navigationDestination(for:)` modifier to specify navigation, and always use `NavigationStack` instead of the old `NavigationView`.
- `iOS 15` If using an image for a button label, always specify text alongside like this: `Button("Tap me", systemImage: "plus", action: myButtonAction)`.
- `iOS 16` When rendering SwiftUI views, always prefer using `ImageRenderer` to `UIGraphicsImageRenderer`.
- `iOS 13` Don’t apply the `fontWeight()` modifier unless there is good reason. If you want to make some text bold, always use `bold()` instead of `fontWeight(.bold)`.
- `iOS 17` Do not use `GeometryReader` if a newer alternative would work as well, such as `containerRelativeFrame()` or `visualEffect()`.
- `iOS 13` When making a `ForEach` out of an `enumerated` sequence, do not convert it to an array first. So, prefer `ForEach(x.enumerated(), id: \.element.id)` instead of `ForEach(Array(x.enumerated()), id: \.element.id)`.
- `iOS 16` When hiding scroll view indicators, use the `.scrollIndicators(.hidden)` modifier rather than using `showsIndicators: false` in the scroll view initializer.
- `iOS 17` Use the newest ScrollView APIs for item scrolling and positioning (e.g. `ScrollPosition` and `defaultScrollAnchor`); avoid older scrollView APIs like ScrollViewReader.
- `iOS 13` Place view logic into view models or similar, so it can be tested.
- `iOS 13` Avoid `AnyView` unless it is absolutely required.
- `iOS 13` Avoid specifying hard-coded values for padding and stack spacing unless requested.
- `iOS 13` Avoid using UIKit colors in SwiftUI code.


## SwiftData instructions

If SwiftData is configured to use CloudKit:

- `iOS 17` Never use `@Attribute(.unique)`.
- `iOS 17` Model properties must always either have default values or be marked as optional.
- `iOS 17` All relationships must be marked optional.


## Project structure

- `iOS 13` Use a consistent project structure, with folder layout determined by app features.
- `iOS 13` Follow strict naming conventions for types, properties, methods, and SwiftData models.
- `iOS 13` Break different types up into different Swift files rather than placing multiple structs, classes, or enums into a single file.
- `iOS 13` Write unit tests for core application logic.
- `iOS 13` Only write UI tests if unit tests are not possible.
- `iOS 13` Add code comments and documentation comments as needed.
- `iOS 13` If the project requires secrets such as API keys, never include them in the repository.
- `iOS 15` If the project uses Localizable.xcstrings, prefer to add user-facing strings using symbol keys (e.g. helloWorld) in the string catalog with `extractionState` set to "manual", accessing them via generated symbols such as  `Text(.helloWorld)`. Offer to translate new keys into all languages supported by the project.


## PR instructions

- `iOS 13` If installed, make sure SwiftLint returns no warnings or errors before committing.


## Xcode MCP

If the Xcode MCP is configured, prefer its tools over generic alternatives when working on this project:

- `iOS 13` `DocumentationSearch` — verify API availability and correct usage before writing code
- `iOS 13` `BuildProject` — build the project after making changes to confirm compilation succeeds
- `iOS 13` `GetBuildLog` — inspect build errors and warnings
- `iOS 13` `RenderPreview` — visually verify SwiftUI views using Xcode Previews
- `iOS 13` `XcodeListNavigatorIssues` — check for issues visible in the Xcode Issue Navigator
- `iOS 13` `ExecuteSnippet` — test a code snippet in the context of a source file
- `iOS 13` `XcodeRead`, `XcodeWrite`, `XcodeUpdate` — prefer these over generic file tools when working with Xcode project files

