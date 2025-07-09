# 📁 RealityAssets

*Apple-Native File Management for Game Development*

![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20iOS-blue)
![Architecture](https://img.shields.io/badge/arch-Apple%20Silicon-green)
![Swift Version](https://img.shields.io/badge/swift-6.0-orange)
![Status](https://img.shields.io/badge/status-Active%20Development-yellow)

**RealityAssets** is a Godot-inspired, SwiftUI-based file and asset manager designed for Apple platforms. It provides a beautiful bottom drawer interface, platform-specific file access, and integrated debugging tools — all as part of the Orchard game engine.

---

## ✨ Key Features

### 🚀 Apple-Native File Management
- SwiftUI architecture with full macOS and iOS compatibility
- `.orchard` document-based project packages
- Platform-aware file access (unsandboxed macOS, sandboxed iOS)
- Future iCloud Drive sync support

### 🎨 Godot-Inspired Interface
- **Bottom drawer UI** instead of traditional sidebars
- Tabbed interface: **FileSystem**, **Debug**, **Search**, **Inspector**
- Expandable tree structure with **color-coded folders**
- Smooth animations, drag gestures, and platform-specific UX

### 🧰 Debug Console System
- Console tab for real-time message viewing
- Filter by message type (error, warning, debug, etc.)
- Search bar for filtering messages
- Floating overlay debugger (independent of drawer)
- Visual theming via SF Symbols and color coding

---

## 📱 Platform Experiences

### macOS
- Unsandboxed file browsing with `FileManager`
- System folder shortcuts (e.g., Desktop, Downloads)
- “Reveal in Finder” and “Open in Terminal” support
- Glassmorphism UI with drag-to-resize drawer
- Native toolbar and hover-based interactions

### iOS
- Sandboxed file access with security-scoped bookmarks
- Files app integration for imports
- Document-based architecture (UIDocument)
- Share sheet support for exporting projects
- Haptic gestures and mobile-first layout

---

## 🏗️ Technical Architecture

### Project Structure
RealityAssets conforms to Apple’s document package model:

```
MyGame.orchard/
├── Project.plist
├── Assets/
│   ├── Models/
│   ├── Textures/
│   └── Audio/
├── Scenes/
│   └── Main.scene
├── Scripts/
│   ├── Swift/
│   ├── Cpp/
│   └── Metal/
└── .orchard/
    ├── cache/
    ├── thumbnails/
    └── debug.log
```

### Bottom Drawer View
```swift
BottomDrawer(
    isVisible: $isDrawerVisible,
    isExpanded: $isDrawerExpanded,
    #if os(macOS)
    drawerHeight: $drawerHeight
    #endif
)
```

- macOS: Fixed-height drawer with drag gestures
- iOS: Percentage-based height with haptics
- Adaptive layout with `DrawerTab` enum

### Drawer Tabs
```swift
enum DrawerTab: String, CaseIterable {
    case filesystem, debugger, search, inspector
}
```

### Current File Hierarchy
```
RealityAssets/
├── App/
│   ├── ContentView.swift
│   └── RealityAssetsApp.swift
├── Assets.xcassets/
├── Sources/Shared/
│   ├── Extensions/HapticFeedback.swift
│   ├── PlatformColor.swift
│   └── Types/TreeSitterStatusTypes.swift
├── Views/
│   ├── BottomDrawer.swift
│   ├── FileSystem.swift
│   └── Console/
│       ├── ConsoleLogEntry.swift
│       ├── ConsoleMessageType.swift
│       ├── DebugConsole.swift
│       ├── FloatingDebugger.swift
│       └── DebuggerOverlay.swift
└── RealityAssets.xcodeproj/
```

---

## 🎯 Part of Orchard

RealityAssets is one of several Apple-native modules in the **Orchard** game engine ecosystem:

- **RealityAssets** — Asset management and project structure
- **RealityStudio** — Visual 3D scene editing
- **DarwinSyntax** — Multi-language script editing
- **OrchardBridge** — Engine coordination layer

---

## 🚧 Development Status

### ✅ Completed
- Cross-platform bottom drawer with animation
- File tree with color-coded nodes
- Debug console system (UI + filtering + overlay)
- File system access (macOS and iOS)
- Initial platform-specific UI polish

### 🔄 In Progress
- `.orchard` document package support
- File import/export system
- Search backend and indexing
- Debug message persistence
- iCloud sync prototype

### 📋 Planned
- Project templates for starter layouts
- Version control metadata
- File operations: rename, delete, copy, move
- Conflict resolution UI
- Asset previews and thumbnails

---

## 🛠️ Build Instructions

### Requirements
- macOS 15 / iOS 18
- Xcode 16+
- Swift 6.0
- Apple Silicon Mac

### Dependencies
- **SwiftUI**
- **FileManager APIs**
- **iCloud Drive / Security Scoped Access**
- **SF Symbols**

---

## 📚 Documentation

Ongoing internal documentation includes:
- File system architecture
- Cross-platform UI components
- Debug console API
- OrchardBridge integration
- Platform differences: macOS vs iOS

---

## 🤝 Contributions

The repository is currently private during development. Feedback, feature requests, and bug reports will be welcomed after public beta.

---

## 📄 License

The license will be announced with the first public beta release. Orchard CE is intended to be FOSS-friendly with proprietary modules only for commercial use.

---

## 🙏 Acknowledgments

- Inspired by **Godot's** File Dock
- Built entirely with **Apple technologies**
- Thanks to the Swift Forums and the Apple developer community

---

<p align="center">
  <i>Built with ❤️ for the Apple ecosystem as part of the Orchard game engine</i>
</p>

<p align="center">
  <a href="#-realityassets">Back to top ↑</a>
</p>
