# Bring your app to Siri

Learn how to use App Intents to expose your app’s functionality to Siri. Understand which intents are already available for your use, and how to create custom intents to integrate actions from your app into the system. We’ll also cover what metadata to provide, making your entities searchable via Spotlight, annotating onscreen references, and much more.

@Metadata {
   @TitleHeading("WWDC24")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc24/10133", purpose: link, label: "Watch Video (21 min)")

   @Contributors {
      @GitHubUser(harrison-heinig)
   }
}

## Key Takeaways
- 🚀 Siri integration enhances app usability across devices.
- 🤖 Apple Intelligence brings natural and contextual interactions.
- 🛠️ Assistant Schemas simplify App Intent development.
- 🔍 Semantic Search boosts in-app content discoverability.

---

## Why Integrate with Siri?
- **Benefits:**
    - Users can take actions with your app anywhere on their device.
    - Quick actions directly from Siri or your app.
- **Frameworks:**
    - **[SiriKit][siri-kit]** (introduced in iOS 10): Use system-provided intents for common actions (e.g., playing music, sending texts).
    - **[App Intents][app-intents]** (introduced in iOS 16): Integrate apps with Siri, Shortcuts, Spotlight, etc., for custom actions.

---

## Apple Intelligence Enhancements
- Powered by Large Language Models (LLMs).
- **Key improvements:**
    - Natural-sounding Siri responses.
    - Contextually relevant and personal interactions.
    - Richer language understanding for natural conversations.
- Automatic benefits for [SiriKit][siri-kit] adopters.

---

## App Intent Domains
- New APIs for connecting apps to Apple Intelligence.
- **Domains:**
    - Collections of AppIntents for specific functionalities (e.g., Books, Camera, Spreadsheets).
    - iOS 18 introduces 12 domains (e.g., Mail, Photos).

*Example:* Darkroom app using `photos.setFilter()` intent: "Apply a cinematic preset to the photo I took of Mary yesterday."

---

## Assistant Schemas
- **Definition:**
    - A schema is a "shape" of an intent that models are trained to understand.
    - Assistant Schemas API simplifies integration.
- **Benefits:**
    - Predefined shapes reduce metadata requirements.
    - Easier to define intents with Swift Macros.
- **Variations88
    - `AssistantIntent`
    - `AssistantEntity`
    - `AssistantEnum`

```swift
@AssistantIntent(schema: .photos.createAlbum)
struct CreateAlbumIntent: OpenIntent {
    var name: String

    func perform() async throws -> some ReturnsValue<AlbumEntity> {
        return .result(value: AlbumEntity(name: name))
    }
}
```

---

## Life-Cycle of a Siri Request
1. User request routed to Apple Intelligence.
2. Models predict the appropriate schema.
3. Request routed to a toolbox of AppIntents grouped by schema.
4. Action performed via AppIntent, and output returned.

---

## Personal Context and Semantic Search
- Siri gains rich understanding of personal context.
- **Features:**
    - In-App Search: Navigate users to app search results.
    - Semantic Search: Understands concepts (e.g., "pets" includes cats, dogs, etc.).
- Use `IndexedEntity` for semantic indexing.

```swift
@AssistantIntent(schema: .system.search)
struct SearchAssetsIntent: AppIntent {
    func perform() -> some IntentResult {
        // Implementation for search
    }
}
```

---

## Additional Resources

- [Bring your app to Siri - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc24-10133-bring-your-app-to-siri)

### Related Videos
- <doc:WWDC24-10134-Whats-new-in-App-Intents>
- <doc:WWDC24-10210-Bring-your-apps-core-features-to-users-with-App-Intents>

[app-intents]: https://developer.apple.com/documentation/appintents/app-intents
[siri-kit]: https://developer.apple.com/documentation/sirikit
