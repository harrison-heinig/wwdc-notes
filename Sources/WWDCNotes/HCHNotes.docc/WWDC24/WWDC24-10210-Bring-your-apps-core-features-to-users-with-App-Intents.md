# Bring your app’s core features to users with App Intents

Learn the principles of the App Intents framework, like intents, entities, and queries, and how you can harness them to expose your app’s most important functionality right where people need it most. Find out how to build deep integration between your app and the many system features built on top of App Intents, including Siri, controls and widgets, Apple Pencil, Shortcuts, the Action button, and more. Get tips on how to build your App Intents integrations efficiently to create the best experiences in every surface while still sharing code and core functionality.

@Metadata {
	@TitleHeading("WWDC24")
	@PageKind(sampleCode)
	@CallToAction(url: "https://developer.apple.com/wwdc24/10210", purpose: link, label: "Watch Video (26 min)")
	
	@Contributors {
		@GitHubUser(harrison-heinig)
	}
}

## Key Takeaways
- 🚀 Elevate app features beyond the app for seamless user experience.
- 🤖 App Intents power Siri, Spotlight, Widgets, and more.
- 🛠️ Share code across multiple system features with App Intents.
- 🔄 Reduce friction by integrating app actions into the device flow.

---

## Reducing Friction and Improving Flow
- **Friction:**
    - Extra steps or barriers that slow down user actions.
- **Flow:**
    - Effortless interaction where the next step is always nearby.
- *Examples:*
    - Spotlight Suggestions for quick app actions.
    - Widgets for glanceable information.
    - Control Center controls for quick access.

---

## What Are App Intents?
- Foundation for system features like Siri, Spotlight, Shortcuts, Widgets, etc.
- **Main responsibilities:**
    - Define core app actions and content for system understanding.
    - Handle communication between the app and system features.
- **Benefits:**
    - Share code across multiple features.
    - Simplify development with fewer APIs to learn.

---

## Components of App Intents
- **[Intents](https://developer.apple.com/documentation/appintents/app-intents)**: Perform actions (verbs) like opening a view or starting a task.
- **[Entities](https://developer.apple.com/documentation/appintents/app-entities)**: Represent objects (nouns) like a Trail or Collection.
- **[App Shortcuts](https://developer.apple.com/documentation/appintents/app-shortcuts)**: Combine intents and entities into actionable sentences.

---

## Example App Intents
### Shortcuts Action

```swift
struct OpenPinnedTrail: AppIntent {
    static let title: LocalizedStringResource = "Open Pinned Trail"
    static let openAppWhenRun = true

    func perform() async throws -> some IntentResult {
        NavigationModel.shared.navigate(to: .pinned)
        return .result()
    }
}
```

### Parameterized Action

```swift
struct OpenTrail: AppIntent, OpenIntent {
    static let title: LocalizedStringResource = "Open Trail"

    @Parameter(title: "Trail")
    var target: TrailEntity

    func perform() async throws -> some IntentResult {
        NavigationModel.shared.navigate(to: target)
    }

    static var parameterSummary: some ParameterSummary {
        Summary("Open \(\.$trail)")
    }
}

struct TrailEntity: AppEntity {
    
    @Property(title: "Trail Name")
    var name: String

    static let typeDisplayRepresentation: TypeDisplayRepresentation = "Trail"

    var displayRepresentation: DisplayRepresentation {
        DisplayRepresentation(title: name), image: Image(named: imageName))
    }

    var id: Trail.ID

    static var defaultQuery = TrailEntityQuery()
}
```


### Home Screen Widget

```swift
struct TrailConditionsWidget: Widget {
    var body: some WidgetConfiguration {
        IntentConfiguration(
            kind: "TrailConditionsWidget",
            intent: TrailWidgetIntent.self) {
            // Implementation
        }
    }
}
```

### Control Center Control

```swift
struct TrailControl: ControlWidget {
    var body: some ControlWidgetConfiguration {
        AppIntentControlConfiguration(
            kind: Self.kind,
            intent: OpenTrail.self
        ) {
            ControlWidgetButton(action: configuration) {
                Image(systemName: configuration.target.glyph)
                Text(configuration.target.name)
            }
        }
    }
}
```

### App Shortcut for Spotlight and Siri

```swift
struct TrailShortcuts: AppShortcutsProvider {
    static var appShortcuts: [AppShortcut] {
        AppShortcut(
            intent: OpenPinnedTrail(),
            phrases: ["Open my pinned trail in \(.applicationName)"],
            shortTitle: "Open Pinned Trail",
            systemImageName: "pin"
        )
    }
}
```

### Advanced Siri Integration

```swift
struct GetPinnedTrail: AppIntent {
    static var title: LocalizedStringResource = "Get Pinned Trail"

    func perform() async throws -> some IntentResult & ProvidesDialog & ShowsSnippetView {
        let pinned = TrailDataManager.shared.pinned
        return .result(
            dialog: "The latest reported condition of \(pinned.name) is \(pinned.currentConditions)",
            view: trailConditionsSnippetView()
        )
    }
}
```

---

## Additional Resources

- [Bring your app's core features to users with App Intents' - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc24-10210-bring-your-apps-core-features-to-users-with-app-intents)

### Related Videos

#### WWDC24
- <doc:WWDC24-10133-Bring-your-app-to-Siri>
- <doc:WWDC24-10176-Design-App-Intents-for-system-experiences>
- <doc:WWDC24-10223-Explore-machine-learning-on-Apple-platformse>

#### WWDC23
- <doc:WWDC23-10103-Explore-enhancements-to-App-Intents>

