---
description: "Workspace architecture rule: App Shell is the reusable foundation for UI layout, settings, persistence, and sync across apps in this repository. Use when planning, implementing, or reviewing app-shell, flutter_app_shell, or consumer app integration work."
---

# App Shell Architecture Rule

- Treat the App Shell as the owning layer for appearance, navigation layout, taskbar, settings panel, and canvas/view structure.
- The App Shell sets the UI kit for Apple platforms: cupertino_native_plus is the intended UI kit path for iOS and macOS. Consumer apps such as Dot Todo follow this Shell directive within the Shell-owned boundaries of layout, navigation, toolbar, sidebar, settings, persistence, and sync.
- Android, Windows, and Web are explicitly separate platform paths with their own UI decisions; the Apple UI kit rule does not extend to them automatically.
- Keep UI technology boundaries explicit: reusable Shell structure belongs to flutter_app_shell; cupertino_native_plus provides the Apple-platform expression inside Shell-owned boundaries and must not introduce a second base architecture for layout, navigation, settings, persistence, or sync.
- Treat the App Shell as the owning layer for reusable persistence and sync primitives, including JSON settings export, folder selection, provider status, sync orchestration, and secret redaction boundaries.
- Consumer apps such as Dot Todo may provide domain data sources, mappers, defaults, labels, and provider-specific adapters, but they should not create a second base architecture for persistence, settings rendering, or cloud sync when the feature is intended to be reusable across apps.
- If a folder picker or cloud connection is user-editable, it must be surfaced through the Shell settings UI first, and consumer apps should only bind or extend that Shell-managed section.
- Secrets such as passwords, tokens, refresh tokens, and API keys must never be exported in Shell JSON settings and must not be stored in plain app settings stores like Hive.
- When planning new reusable storage or sync work, prefer file targets under app-shell/flutter_app_shell before app-specific files; app files should mainly adapt Shell APIs to product-specific screens and data.