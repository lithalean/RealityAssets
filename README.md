# 📁 RealityAssets

*Apple-Native File Management for Game Development*

![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20iOS-blue)
![Architecture](https://img.shields.io/badge/arch-Apple%20Silicon-green)
![Swift Version](https://img.shields.io/badge/swift-6.0-orange)
![Status](https://img.shields.io/badge/status-In%20Development-yellow)

**RealityAssets** is a powerful, Godot-inspired file management system built exclusively for Apple platforms, designed as the asset management backbone for the Orchard game engine. It provides seamless file organization, iCloud synchronization, and cross-platform compatibility with a beautiful bottom drawer interface.

---

## ✨ Key Features

### 🚀 Apple-Native File Management
- Built with **SwiftUI** for native macOS and iOS experiences
- **Document-based architecture** using `.orchard` project packages
- **iCloud Drive integration** for seamless cross-device sync
- **Sandbox-aware** with platform-appropriate file access

### 🎨 Godot-Inspired Interface
- **Bottom drawer design** similar to Godot's file dock
- **Multi-tab system** (FileSystem, Debug, Search, Inspector)
- **Expandable file trees** with color-coded folder types
- **Platform-adaptive layouts** that feel native on each device

### 🔧 Advanced File Operations
- **macOS**: Full unsandboxed file system access with Finder integration
- **iOS**: Sandboxed document management with Files app import
- **Smart file organization** with project-specific folder structures
- **Real-time file watching** and automatic refresh

---

## 📱 Platform Experiences

### macOS
<img width="800" alt="macOS File Management" src="https://github.com/user-attachments/assets/placeholder-macos">

- **Full file system access** with Finder integration
- **Terminal integration** for advanced workflows
- **System folder shortcuts** (Desktop, Downloads, Applications)
- **Drag-and-drop support** for easy file organization
- **Unified toolbar** with native macOS styling

### iOS
<img width="400" alt="iOS File Management" src="https://github.com/user-attachments/assets/placeholder-ios">

- **Sandboxed document management** with security-scoped bookmarks
- **Files app integration** for importing external assets
- **iCloud Drive synchronization** with conflict resolution
- **Touch-optimized interface** with gesture-based interactions
- **Share sheet integration** for project export

---

## 🏗️ Technical Architecture

### Document Package Structure
RealityAssets uses Apple's document package model for optimal integration:

```
MyGame.orchard/              # Document package
├── Project.plist           # Project metadata
├── Assets/
│   ├── Models/            # .usdz, .reality files
│   ├── Textures/          # Native image formats
│   ├── Audio/             # .m4a, .caf files
│   └── Materials/         # .mtl files
├── Scenes/
│   ├── Main.scene         # Custom scene format
│   └── Level1.scene
├── Scripts/
│   ├── Swift/             # .swift files
│   ├── Cpp/               # .cpp/.h files
│   └── Metal/             # .metal shaders
└── .orchard/
    ├── cache/             # System files
    ├── thumbnails/        # Generated previews
    └── index.db           # Search index
```

### Bottom Drawer System
Inspired by Godot's file dock with Apple-native implementation:

```swift
// Cross-platform drawer with adaptive behavior
BottomDrawer(
    isVisible: $isDrawerVisible,
    isExpanded: $isDrawerExpanded,
    drawerHeight: $drawerHeight // macOS only
)
```

- **Smooth animations** with spring physics
- **Drag gestures** for intuitive resizing
- **Material backgrounds** with glassmorphism effects
- **Tab-based navigation** for different file views

### Component Structure
```
├── Core/              # File management logic
├── UI/Drawer/         # Bottom drawer interface
├── FileSystem/        # Platform-specific file ops
└── Platform/          # macOS/iOS adaptations
```

---

## 🎯 Part of Orchard

RealityAssets is a key component of the **Orchard** game engine - a complete Apple-native game development ecosystem:

- **RealityStudio** - 3D scene composition and editing
- **DarwinSyntax** - Multi-language code editor
- **RealityAssets** - File and asset management (this project)
- **OrchardBridge** - Module integration and coordination

---

## 🚧 Development Status

### Currently Working
- ✅ Cross-platform bottom drawer interface
- ✅ Platform-adaptive file system browsing
- ✅ Color-coded file type organization
- ✅ Expandable tree structure navigation
- ✅ Material design with glassmorphism

### In Progress
- 🔄 Document package implementation
- 🔄 iCloud Drive synchronization
- 🔄 File import/export workflows
- 🔄 Search and indexing system
- 🔄 Preview generation

### Planned Features
- 📋 Advanced file operations (move, copy, rename)
- 📋 Version control integration
- 📋 Asset preview and thumbnails
- 📋 Smart folder templates
- 📋 Conflict resolution UI

---

## 🛠️ Building from Source

*Note: Source code is currently in a private repository during initial development.*

### Requirements
- macOS 15.0+ or iOS 18.0+
- Xcode 16.0+
- Swift 6.0
- Apple Silicon Mac (for development)

### Key Dependencies
- **SwiftUI** - Native user interface framework
- **FileManager** - File system operations
- **iCloud Drive APIs** - Cloud synchronization
- **Security Framework** - Sandboxed file access

---

## 📚 Documentation

Comprehensive documentation is being developed:
- **File System Architecture** - Document packages and organization
- **Platform Differences** - macOS vs iOS file access patterns
- **iCloud Integration** - Synchronization and conflict resolution
- **Module Integration** - OrchardBridge compatibility

---

## 🔧 Advanced Features

### Platform-Specific Capabilities

**macOS (Unsandboxed)**:
- Full file system navigation
- Finder integration and "Reveal in Finder"
- Terminal access for command-line operations
- System folder shortcuts
- Advanced file operations

**iOS (Sandboxed)**:
- Document-based app architecture
- Files app integration for imports
- iCloud Drive automatic sync
- Share sheet for project export
- Security-scoped bookmark persistence

### Smart File Organization
- **Automatic categorization** by file type
- **Project templates** for quick setup
- **Recent files tracking** across devices
- **Smart search** with content indexing
- **Duplicate detection** and cleanup

---

## 🤝 Contributing

While the repository is currently private, we welcome feedback and suggestions! Feel free to open issues for:
- Feature requests for file management workflows
- Bug reports (once public beta begins)
- Documentation improvements
- Platform-specific enhancement ideas

---

## 📄 License

RealityAssets will be released under an open-source license (TBD) once it reaches public beta status.

---

## 🙏 Acknowledgments

- [Godot Engine](https://godotengine.org) for file dock inspiration
- [Apple Developer Documentation](https://developer.apple.com) for platform APIs
- [Swift Forums](https://forums.swift.org) community for SwiftUI guidance
- File management best practices from the macOS and iOS communities

---

<p align="center">
  <i>Built with ❤️ for the Apple ecosystem as part of the Orchard game engine</i>
</p>

<p align="center">
  <a href="#-realityassets">Back to top ↑</a>
</p>