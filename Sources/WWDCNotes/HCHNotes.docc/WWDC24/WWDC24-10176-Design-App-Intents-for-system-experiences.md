# Design App Intents for system experiences

App Intents power system experiences in controls, Spotlight, Siri, and more. Find out how to identify the functionality that's best for App Intents, and how to use parameters to make these intents flexible. Learn how to use App Intents to allow people to take action outside your app, and see examples of when to navigate into your app to show contextual information.

@Metadata {
   @TitleHeading("WWDC24")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc24/10176", purpose: link, label: "Watch Video")

   @Contributors {
      @GitHubUser(harrison-heinig)
   }
}

## Key Takeaways

- 🎛️ Surface all app functionality as App Intents, not just habitual tasks.
- 💪 Use flexible parameters to avoid creating multiple intents for similar tasks.
- 🟢 Support toggles for binary states to simplify user interactions.
- 📱 Open your app when appropriate to show changes or results.
- ⏱️ Use dynamic parameters for options that evolve over time.

---

## What should  be an App Intent?

- **Expand Beyond Habitual Tasks:**
    - Previously, App Intents focused on habitual tasks.
    - In iOS 18, all app functionality should be surfaced as App Intents.

- **Avoid Redundancy:**
    - Do not create multiple intents for similar tasks.
        - *Example:* Instead of separate intents for different reminders, use a single intent with a parameter for the reminder type.

- **Focus on Tasks, Not UI Elements:**
    - App Intents should represent tasks, not specific UI actions like tapping a button.
        - *Example:* Instead of "Cancel Button," create an intent for "Save Draft" or "Delete Draft."

- **Background-Friendly Intents:**
    - Create intents that work in the background without requiring app interaction.

---

## Structuring App Intents

- **Parameter Summaries:**
    - Ensure summaries are readable as sentences, regardless of parameter values.
        - *Example:* A camera intent should clearly describe the selected mode (e.g., "Open Camera in Portrait Mode").

- **Built-in Parameters:**
    - Use built-in parameters for simple inputs like numbers, text, or dates.

- **Static Parameters:**
    - Use static parameters for fixed options, like app tabs.

- **Dynamic Parameters:**
    - Use dynamic parameters (e.g., App Entities) for options that change over time, like folders in a notes app.

- **Optional Parameters:**
    - Allow intents to run without requiring all parameters upfront.
        - *Example:* A notes app intent can open the folder picker if no folder is specified.
    - For intents with two states (e.g., on/off), use a toggle as the default parameter.

- **Required Parameters:**
    - Use required parameters only when the intent cannot function without them.
        - *Example:* A search mail intent requires text input.

---

## Additional Resources

- [Design App Intents for system experiences - Community Notes](https://wwdcnotes.com/documentation/wwdcnotes/wwdc24-10176-design-app-intents-for-system-experiences)

### Related Videos

- [What’s new in App Intents](https://developer.apple.com/videos/play/wwdc2024/10133)
- [Bring your app to Siri](https://developer.apple.com//videos/play/wwdc2024/10133)
- [Bring your app’s core features to users with App Intents](https://developer.apple.com//videos/play/wwdc2024/10210)

