# Dive into App Intents

Learn how you can make your app more discoverable and increase app engagement when you use the App Intents framework. We'll take you through the powerful capabilities of this Swift framework, explore the differences between App Intents and SiriKit Intents, and show you how you can expose your app's functionality to the system. We'll also share how you can build entities and queries to create rich App Shortcuts experiences.

@Metadata {
   @TitleHeading("WWDC22")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc22/10032", purpose: link, label: "Watch Video (30 min)")

   @Contributors {
      @GitHubUser(harrison-heinig)
   }
}

## Key Takeaways

- 🚀 Simplify app integration with Siri, Spotlight, and Shortcuts.
- 🛠️ Build powerful, reusable actions with minimal Swift code.

---

## Introduction

- **[App Intents][appintents]**: A new framework for exposing app functionality to the system
- **Key Components:**
    - **[`Intents`][app-intents]:** Actions built into your app, usable throughout the system
    - **[`Entities`][app-entities]:** Represent app concepts
    - **[`App Shortcuts`][app-shortcuts]:** Wrap intents for automation and discoverability

### Benefits of App Intents
- **Voice Control:** Use app features via Siri without setup.
- **Spotlight Integration:** Intents appear in Spotlight search.
- **Focus Filters:** Customize app behavior for specific Focus modes.
- **Shortcuts Integration:** Intents automatically appear in the Shortcuts app.

### Developer Experience
- **Concise:** Simple intents require minimal code.
- **Modern:** Built with Swift
- **Easy Adoption:** No need for extensions or re-architecting.
- **Maintainable:** Code serves as the single source of truth.

---

## Building Intents

1. **Basic Intent:**
    - Define a Swift struct conforming to `AppIntent`.
    - Implement the `perform` method.
    - Add metadata like a localized title.

```swift
struct OpenCurrentlyReading: AppIntent {
    static var title: LocalizedStringResource = "Open Currently Reading"

    @MainActor
    func perform() async throws -> some IntentResult {
        Navigator.shared.openShelf(.currentlyReading)
        return .result()
    }

    static var openAppWhenRun: Bool = true
}
```

2. **Parameterized Intent:**
    - Use enums conforming to `AppEnum` for fixed parameters.
    - Use `@Parameter` for inputs.
    - Add `ParameterSummary` for better Shortcuts UI.

```swift
struct OpenShelf: AppIntent {
    static var title: LocalizedStringResource = "Open Shelf"

    @Parameter(title: "Shelf")
    var shelf: Shelf

    @MainActor
    func perform() async throws -> some IntentResult {
        Navigator.shared.openShelf(shelf)
        return .result()
    }

    static var parameterSummary: some ParameterSummary {
        Summary("Open \(\.$shelf)")
    }

    static var openAppWhenRun: Bool = true
}
```

3. **Dynamic Entities:**
    - Use `AppEntity` for dynamic, user-defined values.
    - Implement `EntityQuery` for retrieving entities.
    - *Example:* `BookEntity` with a query to fetch books.

```swift
struct BookEntity: AppEntity, Identifiable {
    var id: UUID

    var displayRepresentation: DisplayRepresentation { "\(title)" }

    static var typeDisplayRepresentation: TypeDisplayRepresentation = "Book"

    static var defaultQuery = BookQuery()
}
```

```swift
struct BookQuery: EntityQuery {
    func entities(for identifiers: [UUID]) async throws -> [BookEntity] {
        identifiers.compactMap { identifier in
            Database.shared.book(for: identifier)
        }
    }
}
````

4. **Combining Intents:**
    - Return values from intents to chain them.
    - Use `openIntent` for seamless transitions between intents.

```swift
struct AddBook: AppIntent {
    static var title: LocalizedStringResource = "Add Book"

    @Parameter(title: "Title")
    var title: String

    @Parameter(title: "Author Name")
    var authorName: String?

    @Parameter(title: "Recommended By")
    var recommendedBy: String?

    func perform() async throws -> some IntentResult & ReturnsValue<BookEntity> & OpensIntent {
        guard var book = await BooksAPI.shared.findBooks(named: title, author: authorName).first else {
            throw Error.notFound
        }
        book.recommendedBy = recommendedBy
        Database.shared.add(book: book)

        return .result(
            value: book,
            openIntent: OpenBook(book: book)
        )
    }

    enum Error: Swift.Error, CustomLocalizedStringResourceConvertible {
        case notFound

        var localizedStringResource: LocalizedStringResource {
            switch self {
                case .notFound: return "Book Not Found"
            }
        }
    }
}
```

### Advanced Features

- **Properties:**
    - Add additional information to entities using @Property.
    - Enable filtering and sorting in Shortcuts.

- **Property Queries:**
    - Use EntityPropertyQuery for advanced filtering.
    - Define query properties and sorting options.
    - *Example:* Find books by author or publication date.

- **User Interactions:**
    - Dialog: Provide text/voice feedback.
    - Snippets: Add visual feedback using SwiftUI views.
    - Request Value: Prompt users for missing or ambiguous values.
    - Disambiguation: Ask users to choose between multiple options.
    - Confirmation: Verify parameter values or intent results.

---

## Architecture

- **In-App Implementation:**
    - Simplest approach.
    - Higher memory limits and foreground/background support.

- **Extension Implementation:**
    - Lightweight and independent of the app process.
    - Ideal for Focus intents.

---

## Additional Resources
- [Dive into App Intents - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc22-10032-dive-into-app-intents)

[appintents]: https://developer.apple.com/documentation/appintents
[app-shortcuts]: https://developer.apple.com/documentation/appintents/app-shortcuts
[app-intents]: https://developer.apple.com/documentation/appintents/app-intents
[app-entities]: https://developer.apple.com/documentation/appintents/app-entities
[IntentResult]: https://developer.apple.com/documentation/appintents/intentresult
[IntentError]: https://developer.apple.com/documentation/appintents/intenterror
[appintent/perform()]: https://developer.apple.com/documentation/appintents/appintent/perform()
[AppEnum]: https://developer.apple.com/documentation/appintents/appenum
[IntentParameter]: https://developer.apple.com/documentation/appintents/intentparameter
[AppEntity]: https://developer.apple.com/documentation/appintents/appentity
[openentityintent]: https://developer.apple.com/documentation/appintents/openentityintent
[entity-queries]: https://developer.apple.com/documentation/appintents/entity-queries
[property-comparators]: https://developer.apple.com/documentation/appintents/property-comparators
[EntityPropertyQuery]: https://developer.apple.com/documentation/appintents/entitypropertyquery
[requestDisambiguation(among:dialog:)]: https://developer.apple.com/documentation/appintents/intentparameter/requestdisambiguation(among:dialog:)
[requestConfirmation(for:dialog:)]: https://developer.apple.com/documentation/appintents/intentparameter/requestconfirmation(for:dialog:)
[IntentDialog]: https://developer.apple.com/documentation/appintents/intentdialog
[finished(dialog:view:)]: https://developer.apple.com/documentation/appintents/intentperformresult/finished(dialog:view:)-5aejs
[snippetviewtype]: https://developer.apple.com/documentation/appintents/intentperformresult/snippetviewtype
[openAppWhenRun]: https://developer.apple.com/documentation/appintents/appintent/openappwhenrun-3u9y8
