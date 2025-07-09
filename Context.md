# RealityAssets — AI Context Document

**Date**: January 2025  
**Status**: Active Development - Bottom Drawer Implementation Phase  
**Purpose**: Accurate context for AI systems working with RealityAssets codebase

---

## 🎯 Project Identity

**RealityAssets** is a SwiftUI-based file management system for the Orchard game engine, inspired by Godot's file dock.

**Key Facts:**
- Part of Orchard (not standalone file manager)
- Cross-platform (macOS and iOS)
- Godot-inspired bottom drawer interface
- Apple-native document package approach
- Platform-aware file access (unsandboxed macOS, sandboxed iOS)

---

## 🏗️ Current Architecture

### Directory Structure
```
RealityAssets/
├── App/
│   ├── ContentView.swift              # Platform router & main UI
│   └── RealityAssetsApp.swift         # Main app with settings
├── Core/
│   └── (Future file management logic)
├── UI/
│   ├── BottomDrawer.swift             # Combined drawer implementation
│   └── FileSystem.swift              # Platform-specific file browsers
├── Sources/
│   └── Shared/
│       ├── Components/
│       ├── Extensions/
│       │   └── HapticFeedback.swift   # iOS haptic feedback
│       └── PlatformColor.swift
└── Assets.xcassets/
    └── (App icons and resources)
```

### Key Components Status
- **BottomDrawer.swift**: ✅ Complete - Cross-platform drawer with animations
- **FileSystem.swift**: ✅ Complete - Platform-specific file browsers
- **ContentView.swift**: ✅ Complete - Welcome screens with drawer integration
- **RealityAssetsApp.swift**: ✅ Complete - App structure with settings

---

## 🔧 Current Implementation Details

### Bottom Drawer System

#### Cross-Platform Design
```swift
struct BottomDrawer: View {
    @Binding var isVisible: Bool
    @Binding var isExpanded: Bool
    #if os(macOS)
    @Binding var drawerHeight: CGFloat  // macOS uses fixed height
    #endif
    // iOS uses percentage-based height
}
```

**macOS Implementation**:
- Fixed height with manual resizing via `drawerHeight` binding
- Drag gestures for expand/collapse
- Material background with glass effects
- Positioned at bottom with horizontal padding

**iOS Implementation**:
- Percentage-based height (70% when expanded, 240pt when collapsed)
- Slide-up gesture with haptic feedback
- Rounded corners with continuous style
- Ignores safe area for full-screen feel

#### Tab System
```swift
enum DrawerTab: String, CaseIterable {
    case filesystem = "FileSystem"
    case debugger = "Debug"
    case search = "Search"
    case inspector = "Inspector"
}
```

### File System Implementation

#### Platform Differences

**macOS (Unsandboxed)**:
- Full file system access with `FileManager`
- System folder shortcuts (Desktop, Downloads, Applications)
- Finder integration ("Reveal in Finder", "Open Terminal")
- Project structure: `project://Assets/`, `project://Scenes/`
- Additional tools: Terminal access, external folder navigation

**iOS (Sandboxed)**:
- Document-based with security-scoped bookmarks
- Limited to Documents directory and iCloud Drive
- Files app integration for imports
- Project structure: `Documents://Assets/`, `iCloud://Projects/`
- Share sheet for project export

#### File Tree Structure
```swift
struct FileSystemItem: View {
    enum FileSystemType {
        // Folders
        case folder, assets, scenes, scripts, build, documentation
        // Files  
        case file, script, scene, config, image, model, audio
        
        var color: Color // Color-coded by type
        var icon: String // SF Symbol for each type
        var canExpand: Bool // Whether it's expandable
    }
}
```

**Color Coding**:
- Assets: Green
- Scenes: Pink  
- Scripts: Yellow
- Build: Red
- Documentation: Blue
- Files: Secondary/context-specific

---

## 🎨 UI/UX Architecture

### Layout Philosophy
**Godot-inspired bottom drawer** instead of traditional sidebar:
- Main content area takes full screen
- File browser slides up from bottom
- Tabs for different file views
- Expandable/collapsible with smooth animations

### Platform Adaptations

#### macOS
- Window-based app with unified toolbar
- Fixed drawer height with drag resize
- Glass material backgrounds
- Hover effects and cursor changes
- Menu bar integration with keyboard shortcuts

#### iOS  
- Navigation-based with toolbar
- Percentage-based drawer sizing
- Haptic feedback on interactions
- Touch-optimized gestures
- Share sheet integration

### Visual Design
- **Glass morphism effects** with material backgrounds
- **Smooth spring animations** for all transitions
- **Color-coded file types** for quick identification
- **Consistent spacing** using `GlassConstants`
- **Platform-appropriate styling** (NSColor vs UIColor)

---

## 📱 Current User Experience

### Welcome Screen
**macOS**: Desktop-style with toolbar and large buttons
**iOS**: Mobile-friendly with navigation and stacked buttons

### File Management Flow
1. User opens app to welcome screen
2. Clicks "Toggle Files" or "Files" button
3. Bottom drawer slides up with file browser
4. User can switch between filesystem/debug/search/inspector tabs
5. Expandable tree navigation with color-coded folders
6. Platform-specific actions (Finder on macOS, Share on iOS)

### Project Structure (Planned)
```
MyGame.orchard/              # Document package
├── Project.plist           # Project metadata
├── Assets/
│   ├── Models/            # .usdz, .reality
│   ├── Textures/          # .heic, .png
│   └── Audio/             # .m4a, .caf
├── Scenes/
│   └── Main.scene         # Custom format
├── Scripts/
│   ├── Swift/             # .swift files
│   ├── Cpp/               # .cpp files
│   └── Metal/             # .metal shaders
└── .orchard/
    ├── cache/             # System files
    └── thumbnails/        # Generated previews
```

---

## 🚧 Current Development State

### ✅ What's Working
- Cross-platform bottom drawer with animations
- Platform-specific file system browsing
- Color-coded file type organization
- Expandable tree structure navigation
- Tab system for different views
- Welcome screens with drawer integration
- Settings system (macOS) with preferences

### ⚠️ Known Issues
- **Document package system**: Not yet implemented
- **iCloud synchronization**: Placeholder only
- **File operations**: Limited to display/navigation
- **Search functionality**: Not implemented
- **Preview generation**: Not implemented

### 🔄 In Progress
- Document package implementation
- Real file system operations
- iCloud Drive integration
- File import/export workflows
- Search and indexing system

---

## 🛠️ Development Guidelines

### Code Patterns
- Use `#if os(macOS)` for platform-specific code
- Material backgrounds with glass effects
- Spring animations for smooth transitions
- Color-coded file types with SF Symbols
- Consistent spacing using `GlassConstants`

### Adding Features
1. **New file types**: Add to `FileSystemItem.FileSystemType` enum
2. **New drawer tabs**: Add to `DrawerTab` enum and implement panel
3. **Platform features**: Use conditional compilation
4. **File operations**: Implement in platform-specific panels

### Integration Points
- **OrchardBridge**: Will coordinate with other modules
- **RealityStudio**: Will receive file selection events
- **DarwinSyntax**: Will handle script file editing
- **iCloud Drive**: For cross-device synchronization

---

## 🔍 Technical Challenges

### Current Blockers
1. **Document package implementation**: Need NSDocument/UIDocument integration
2. **iCloud synchronization**: Conflict resolution and status monitoring
3. **File operations**: Move, copy, rename, delete functionality
4. **Search indexing**: Content-based search across project files

### Architecture Decisions
- **Document packages** over virtual filesystem (Apple-native)
- **Bottom drawer** over sidebar (Godot-inspired)
- **Platform-specific implementations** over lowest-common-denominator
- **Material design** over flat design (modern Apple aesthetic)

---

## 🔗 Module Integration

### Standalone Capabilities
- Project file organization
- Cross-platform file browsing
- iCloud synchronization
- File import/export

### OrchardBridge Integration
```swift
// Future integration points
protocol OrchardModule {
    func handle(_ message: ModuleMessage) -> ModuleResponse
}

// Example messages
case .fileSelected(URL)
case .projectOpened(ProjectDocument)
case .assetImported(AssetInfo)
```

---

## 📝 Common Development Tasks

### Testing the Current Implementation
1. Run app on macOS and iOS
2. Click "Toggle Files" or "Files" button
3. Verify drawer slides up smoothly
4. Test tab switching between filesystem/debug/search/inspector
5. Verify file tree expansion/collapse
6. Test drag gestures for drawer resizing

### Debugging Issues
- **Drawer not appearing**: Check `isVisible` binding
- **Animation problems**: Verify spring animation values
- **Platform differences**: Test on both macOS and iOS
- **File tree issues**: Check `expandedFolders` state management

---

*This document reflects the actual current state as of January 2025. The focus is on the bottom drawer interface and platform-specific file browsing, with document packages and iCloud sync as the next major development phase.*