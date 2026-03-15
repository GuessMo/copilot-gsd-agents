---
name: app-shell-desktop-ui
description: "Use when planning, implementing, or reviewing shared App Shell desktop UI work, macOS host-chrome boundaries, or Apple-specific widget-package evaluations such as macos_ui or cupertino_native_plus."
---

# App Shell Desktop UI

Use this skill when work touches shared desktop structure, macOS host chrome, or decisions about Apple-specific widget packages.

## Goal

Keep the App Shell as the reusable foundation for:

- layout, navigation, taskbar, settings panel, and view structure
- desktop interaction rules, panel composition, and reusable UI boundaries
- package-neutral Shell contracts that consumer apps can bind without introducing a second base architecture

In this repository, the current macOS host chrome in app/ may use macos_ui during the transition to cupertino_native_plus, while flutter_app_shell remains the reusable Shell foundation.

The App Shell sets the UI kit for Apple platforms: cupertino_native_plus is the intended UI kit path for iOS and macOS. Consumer apps such as Dot Todo follow this Shell directive within Shell-owned boundaries. Android, Windows, and Web are explicitly separate platform paths with their own UI decisions and are not covered by this Apple UI kit rule.

## Required Checks

1. Confirm whether the UI concern belongs to reusable Shell structure or only to app-level host chrome.
2. Confirm whether a proposed package changes host chrome, navigation, toolbar, sidebar, settings structure, or other Shell-owned responsibilities.
3. If an Apple-specific package is proposed, confirm that it follows the Shell-set Apple UI kit path (cupertino_native_plus for iOS and macOS) and does not introduce a second base architecture for Shell-owned concerns.
4. Confirm that Android, Windows, and Web paths are not affected by Apple-platform decisions; they remain separate and require their own explicit UI decisions.

## Package Rules

1. macos_ui may remain the macOS host-chrome layer in app/.
2. flutter_app_shell stays the owner of reusable structure and must not be replaced by a second Shell framework.
3. cupertino_native_plus is the Shell-set Apple UI kit for iOS and macOS. Consumer migrations must stay within Shell-owned boundaries and must not create a second base architecture for layout, navigation, settings, persistence, or sync.
4. Do not move persistence, sync, settings ownership, or navigation responsibility into an Apple-specific widget package outside the Shell contract.

## Planning Pattern

When creating plans:

- document host-chrome versus Shell ownership first
- rule out full migrations unless the repository has made a new architecture decision
- scope Apple-platform work to the Shell-prescribed cupertino_native_plus path for iOS and macOS; keep Android, Windows, and Web on their own separate paths
- require a rollback path that removes package-specific bindings cleanly if the migration is reversed
- define verify steps for macOS and iOS rendering, keyboard/focus, semantics, and non-Apple platform isolation

## Review Checklist

- Does the change preserve App Shell ownership of reusable UI structure?
- Is macos_ui still treated as host chrome rather than as the reusable Shell itself?
- Does any cupertino_native_plus usage follow the Shell-set Apple UI kit path and stay within Shell-owned boundaries?
- Is there no second UI base architecture introduced for layout, navigation, settings, persistence, or sync?
- Are Android, Windows, and Web paths explicitly kept separate from the Apple UI kit rule?
- Are accessibility, keyboard interaction, and non-Apple platform paths still protected?