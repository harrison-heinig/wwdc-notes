# What’s new in App Intents

Learn about improvements and refinements to App Intents, and discover how this framework can help you expose your app’s functionality to Siri and all-new features. We’ll show you how to make your entities more meaningful to the platform with the Transferable API, File Representations, new IntentFile APIs, and Spotlight Indexing, opening up powerful functionality in Siri and the Shortcuts app. Empower your intents to take people deep into your app with URL Representable Entities. Explore new techniques to model your entities and intents with new APIs for error handling and union values

@Metadata {
   @TitleHeading("WWDC24")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc24/10134", purpose: link, label: "Watch Video")

   @Contributors {
      @GitHubUser(harrison-heinig)
   }
}

## Key Takeaways: What's New in App Intents
- 🔍 Index app entities in Spotlight with `IndexedEntity` for better search and Siri integration.
- 📄 Use `Transferable` to convert app entities into shareable formats like PDFs or images.
- 🌐 Add deep linking with `URLRepresentableEntity` for universal links.
- 🛠️ Simplify parameters with `UnionValue` for flexible input types.
- 🚀 Improved framework support allows App Intents to reference entities across modules.

---

## What's New in App Intents

### Spotlight Integration
- **Index App Entities:**
   - Use the new `IndexedEntity` protocol to index app entities in Spotlight.
   - Enables semantic search and Siri integration for app entities.

```swift
struct TrailEntity: AppEntity, IndexedEntity {
    static var typeDisplayName: LocalizedStringResource = "Trail"
    
    var id: String
    var name: String
    var location: String
    
    var attributeSet: CSSearchableItemAttributeSet {
        let attributes = CSSearchableItemAttributeSet()
        attributes.title = name
        attributes.contentDescription = location
        return attributes
    }
}
```

- **Indexing Example:**
   - Call `CSSearchableIndex.default().indexAppEntities()` to index entities.

```swift
CSSearchableIndex.default().indexAppEntities([TrailEntity(id: "1", name: "Sunset Trail", location: "California")])
```

- **Set Priorities:**
   -  Use priority to rank entities in Spotlight search results.

### Transferable API
- **Convert Entities:**
   - Use `Transferable` to convert app entities into formats like PDFs, images, or rich text.

```swift
struct ActivitySummary: AppEntity, Transferable {
    static var typeDisplayName: LocalizedStringResource = "Activity Summary"
    
    static var transferRepresentation: some TransferRepresentation {
        DataRepresentation(exportedContentType: .rtf) { summary in
            summary.toRichText()
        }
        FileRepresentation(exportedContentType: .png) { summary in
            summary.toPNG()
        }
    }
}
```

- **Usage in Shortcuts:**
   - Automatically converts entities to the requested format in Shortcuts.

### URL Representations
- **Deep Linking:**
   - Use `URLRepresentableEntity` to add universal links for app entities.

```swift
struct TrailEntity: AppEntity, URLRepresentableEntity {
    static var typeDisplayName: LocalizedStringResource = "Trail"
    
    var id: String
    
    static var urlRepresentation: URLRepresentation {
        URLRepresentation(template: "https://example.com/trails/\(id)")
    }
}
```

- **Open URLs:**
   - Use OpenURLIntent to open URLs directly from App Intents.

### UnionValue for Flexible Parameters
- **Combine Input Types:**
   - Use `UnionValue` to accept multiple types for a single parameter.

```swift
enum DayPassType: UnionValue {
    case trail(TrailEntity)
    case park(ParkEntity)
}

struct BuyDayPass: AppIntent {
    @Parameter(title: "Pass Type")
    var passType: DayPassType
    
    func perform() async throws -> some IntentResult {
        switch passType {
        case .trail(let trail):
            // Handle trail pass
        case .park(let park):
            // Handle park pass
        }
        return .result()
    }
}
```

### Developer Experience Improvements
- **Automatic Parameter Titles:**
   - Xcode generates parameter titles automatically based on property names.
- **Framework Support:**
   - App Intents can now reference entities defined in frameworks.

---

## Additional Resources

- [What's new in App Intents - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc24-10134-whats-new-in-app-intents)

### Related Videos

#### WWDC24
- <doc:WWDC24-10133-Bring-your-app-to-Siri>
- <doc:WWDC24-10210-Bring-your-apps-core-features-to-users-with-App-Intents>
- <doc:WWDC24-10176-Design-App-Intents-for-system-experiences>
- <doc:WWDC24-10223-Explore-machine-learning-on-Apple-platforms>

#### WWDC23

- <doc:WWDC23-10103-Explore-enhancements-to-App-Intents>

#### WWDC22

- <doc:WWDC22-10062-Meet-Transferable>

#### WWDC21

- <doc:WWDC21-10098-Showcase-app-data-in-Spotlight>
