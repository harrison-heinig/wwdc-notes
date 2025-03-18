# Explore enhancements to App Intents

Bring your widgets to life with App Intents! Explore the latest updates and learn how you can take advantage of dynamic options and user interactivity to build better experiences for your App Shortcuts. We’ll share how you can integrate with Apple Pay, structure your code more efficiently, and take your Shortcuts app integration to the next level.

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10103", purpose: link, label: "Watch Video (29 min)")

   @Contributors {
      @GitHubUser(harrison-heinig)
   }
}

## Key Takeaways
- 🛠️ Simplify widget configuration with App Intents, no Intent Definition Files needed.
- 🚀 Add interactivity to widgets with tappable buttons and toggles.
- 🔄 Migrate SiriKit widgets to App Intents easily with Xcode tools.
- 📚 Use IntentParameterDependency for dynamic, context-aware options.
- ⚡ Improve performance with modular App Intents in frameworks.

---

## Widgets and App Intents

- **Widget Configuration:**
    - Use `AppIntentConfiguration` instead of `IntentConfiguration`.
    - Define parameters directly in the widget extension using `WidgetConfigurationIntent`.

```swift
@main
struct UpNextWidget: Widget {
    let kind: String = "UpNext"
    
    var body: some WidgetConfiguration {
        AppIntentConfiguration(
            kind: kind,
            intent: UpNextConfiguration.self,
            provider: Provider()
        ) { entry in
            UpNextWidgetView(entry: entry)
            }
        }
    }

    struct UpNextConfiguration: AppIntent, WidgetConfigurationIntent {
    static var title: LocalizedStringResource = "Up Next"

    @Parameter(title: "Example")
    var example: Example
}
```

- **Dynamic Options:**
    - Provide dynamic options for parameters using `DynamicOptionsProvider` or `EntityQuery`.
    - Use `IntentParameterDependency` to filter options based on other parameter values.

- **Interactive Widgets:**
    - Add tappable buttons and toggles to widgets using App Intents.
    
```swift
struct SetAlarm: AppIntent {
    static var title: LocalizedStringResource = "Set Alarm"

    @Parameter(title: "Bus Stop")
    var busStop: BusStop

    func perform() async throws -> some IntentResult {
        AlarmManager.shared.addAlarm(forTime: arrivalTime)
        return .result()
    }
}

struct NextBusView: View {
    var body: some View {
        Button(intent: SetAlarm(arrivalTime: arrivalTime)) {
            Text(arrivalTime.asString)
        }
    }
}
```

---

## Integrations

- **Apple Pay Integration:**
    - Initiate Apple Pay transactions directly in the perform method using `PKPaymentRequest` and `PKPaymentAuthorizationController`.

- **Shortcuts Integration:**
    - Progress Reporting:
        - Use `ProgressReportingIntent` to show progress for long-running actions.
    - Find Actions:
        - Use `EnumerableEntityQuery` for small datasets or `EntityPropertyQuery` for large datasets.

---

## Additional Resources

- [Explore enhancements to App Intents - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc23-10103-explore-enhancements-to-app-intents)

### Related Videos

- [Dive into App Intents](https://developer.apple.com/wwdc22/10032)
- [Bring widgets to life](https://developer.apple.com/videos/play/wwdc2023/10028)
- [Design Shortcuts for Spotlight](https://developer.apple.com/videos/play/wwdc2023/10193)
- [Spotlight your app with App Shortcuts](https://developer.apple.com/videos/play/wwdc2023/10102)
