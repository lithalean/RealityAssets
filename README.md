# 📁 RealityAssets

*Apple-Native File Management for Game Development*

![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20iOS-blue)
![Architecture](https://img.shields.io/badge/arch-Apple%20Silicon-green)
![Swift Version](https://img.shields.io/badge/swift-6.0-orange)
![Status](https://img.shields.io/badge/status-Active%20Development-yellow)

**RealityAssets** is a Godot-inspired, SwiftUI-based file and asset manager designed for Apple platforms. It provides a beautiful bottom drawer interface with real file system access, smart file type detection, integrated debugging tools, and platform-optimized experiences — all as part of the Orchard game engine.

---

## ✨ Key Features

### 🚀 Real File System Management
- **Live file browsing** with actual system files and directories
- **Smart file type detection** using UTI and extensions
- **Platform-aware access**: Full filesystem on macOS, sandboxed on iOS
- **Lazy loading** for efficient directory traversal
- **File metadata display**: Size, modification date, type icons

### 🎨 Godot-Inspired Interface
- **Bottom drawer UI** that slides up from bottom
- **Four functional tabs**: FileSystem, Debug, Search, Inspector
- **Expandable tree structure** with color-coded files and folders
- **Smooth spring animations** with platform-specific gestures
- **Glass morphism design** with material backgrounds

### 🔍 Advanced File Operations
- **Sorting options**: Name ↑↓, Date ↑↓, Type ↑
- **Hidden files toggle** for system file visibility
- **Context menus** with platform actions (macOS)
- **File selection** with visual highlighting
- **Double-click to open** files in default apps (macOS)

### 🧰 Integrated Debug Console
- **Real-time message logging** with timestamps
- **Type-based filtering**: Errors, warnings, info, debug
- **Search functionality** across messages and sources
- **Auto-scroll option** to follow latest messages
- **Clear console** with one click
- **Floating debugger** option for overlay mode

---

## 📱 Platform Experiences

### macOS
<img width="800" alt="macOS File Management" src="https://github.com/user-attachments/assets/placeholder-macos">

- **Full file system access** — Browse entire filesystem
- **System shortcuts** — Home, Desktop, Documents, Downloads, Applications
- **Finder integration** — "Reveal in Finder" context menu
- **Terminal access** — "Open in Terminal" for directories
- **Direct file opening** — Files launch in default applications
- **Hover effects** — Visual feedback on mouse over
- **Drag to resize** — Adjustable drawer height

### iOS
<img width="400" alt="iOS File Management" src="https://github.com/user-attachments/assets/placeholder-ios">

- **Sandboxed access** — App Documents and iCloud containers
- **Document picker** — Import files and folders from Files app
- **Security-scoped bookmarks** — Persistent access to user selections
- **Touch-optimized** — Tap to expand, swipe gestures
- **Haptic feedback** — Physical response to interactions
- **Adaptive layout** — Optimized for iPhone and iPad

---

## 🏗️ Technical Architecture

### Core Components
```
RealityAssets/
├── Views/
│   ├── BottomDrawer.swift         # Main drawer container
│   ├── FileSystem/
│   │   ├── FileSystemModel.swift  # File data model & view model
│   │   ├── FileSystemView.swift   # File browser UI
│   │   └── FilePermissionsHelper.swift # iOS permissions
│   └── Console/
│       ├── DebugConsole.swift     # Debug console view
│       └── FloatingDebugger.swift # Floating overlay
├── Sources/Shared/
│   ├── GlassConstants.swift       # UI constants
│   └── PlatformColor.swift        # Cross-platform colors
└── App/
    ├── ContentView.swift          # Main app structure
    └── RealityAssetsApp.swift     # App lifecycle
```

### File System Model
```swift
struct FileSystemItem: Identifiable {
    let url: URL
    let name: String
    let isDirectory: Bool
    let fileType: FileType
    let size: Int64?
    let modificationDate: Date?
    var children: [FileSystemItem]?  // Lazy loaded
}

class FileSystemViewModel: ObservableObject {
    @Published var rootItems: [FileSystemItem]
    @Published var expandedFolders: Set<URL>
    @Published var selectedItem: FileSystemItem?
}
```

### Smart File Type Detection
```swift
enum FileType {
    // Directories
    case folder, assetsFolder, scenesFolder, scriptsFolder
    
    // Files by category
    case swiftFile, cppFile, sceneFile, imageFile, 
         modelFile, audioFile, documentFile, archiveFile
    
    // Smart detection via UTI + extension
    static func fromUTI(_ uti: String, fileName: String) -> FileType
    static func fromDirectoryName(_ name: String) -> FileType
}
```

### Bottom Drawer System
```swift
BottomDrawer(
    isVisible: $isDrawerVisible,
    isExpanded: $isDrawerExpanded,
    #if os(macOS)
    drawerHeight: $drawerHeight  // Fixed height on macOS
    #endif
)
```

---

## 🎯 Part of Orchard

RealityAssets is a key component of the **Orchard** game engine ecosystem:

- **RealityAssets** — File management and project organization (this project)
- **RealityStudio** — 3D scene composition and editing
- **DarwinSyntax** — Multi-language code editor
- **OrchardBridge** — Module integration and coordination

---

## 🚧 Development Status

### ✅ Working Features
- ✨ **Real file browsing** on both platforms
- 📂 **Smart file type detection** with UTI support
- 🎨 **Color-coded icons** for all file types
- 🔍 **Sorting and filtering** options
- 📱 **Platform-specific features** (Finder/Terminal on macOS, Document Picker on iOS)
- 🐛 **Debug console** with filtering and search
- 🎯 **File selection** and metadata display
- 🚀 **Lazy loading** for performance
- 🔐 **Security-scoped bookmarks** on iOS

### 🔄 In Progress
- 📦 Document package implementation (`.orchard` files)
- ☁️ iCloud Drive synchronization
- 🔧 File operations (rename, move, copy, delete)
- 🔎 Search backend implementation
- 📊 Inspector panel functionality

### 📋 Planned Features
- 🎨 Asset preview generation
- 📋 Project templates
- 🔄 Version control integration
- 🤝 Conflict resolution UI
- 📱 iPad-optimized layout
- 🎮 Game asset importers

---

## 🛠️ Building from Source

### Requirements
- macOS 15.0+ or iOS 18.0+
- Xcode 16.0+
- Swift 6.0
- Apple Silicon Mac (for development)

### Build Steps
```bash
# Clone the repository
git clone [repository-url]

# Open in Xcode
cd RealityAssets
open RealityAssets.xcodeproj

# Select target (macOS or iOS)
# Build and run (⌘R)
```

### Key Dependencies
- **SwiftUI** — Native UI framework
- **FileManager** — File system operations
- **UniformTypeIdentifiers** — File type detection
- **Security Framework** — iOS sandboxed access

---

## 📚 Documentation

Comprehensive documentation includes:
- **Architecture Overview** — Component relationships
- **File System Guide** — Platform differences and capabilities
- **Debug Console API** — Integration with logging systems
- **UI Components** — Reusable views and modifiers
- **Platform Adaptations** — macOS vs iOS specifics

---

## 🔧 Usage Examples

### Basic File Browsing
```swift
// The file browser is integrated into the bottom drawer
BottomDrawer(isVisible: $showDrawer, isExpanded: $expanded)

// Files are automatically loaded based on platform
// macOS: Full system access
// iOS: Sandboxed + document picker
```

### Debug Console Integration
```swift
// Log messages to the debug console
DebugConsoleView.addMessage(
    "Asset loaded successfully",
    type: .success,
    source: "AssetManager"
)
```

### File Type Detection
```swift
// Automatic file type detection
let fileType = FileType.fromUTI(uti, fileName: "model.usdz")
// Returns: .modelFile with cyan color and cube icon
```

---

## 🤝 Contributing

While the repository is currently private during initial development, we welcome:
- 🐛 Bug reports and issue tracking
- 💡 Feature requests and suggestions
- 📖 Documentation improvements
- 🎨 UI/UX enhancement ideas

---

## 📄 License

RealityAssets will be released under an open-source license (TBD) once it reaches public beta status. The goal is to support both open-source game development and commercial use cases.

---

## 🙏 Acknowledgments

- [Godot Engine](https://godotengine.org) for the file dock inspiration
- [Apple Developer Documentation](https://developer.apple.com) for excellent platform APIs
- [Swift Forums](https://forums.swift.org) community for SwiftUI guidance
- The macOS and iOS developer communities for best practices

---

## 📸 Screenshots

### macOS Experience
- Full file system browsing
- Finder integration
- Context menus
- Hover effects

### iOS Experience  
- Document-based access
- Files app integration
- Touch gestures
- Haptic feedback

---

<p align="center">
  <i>Built with ❤️ for the Apple ecosystem as part of the Orchard game engine</i>
</p>

<p align="center">
  <a href="#-realityassets">Back to top ↑</a>
</p>