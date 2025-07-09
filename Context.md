# RealityAssets — AI Context Document

**Date**: January 2025  
**Status**: Active Development - Debug Console Integration Complete  
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
- Integrated debug console system

---

## 🏗️ Current Architecture

### Directory Structure
```
RealityAssets/
├── App/
│   ├── ContentView.swift              # Platform router & main UI
│   └── RealityAssetsApp.swift         # Main app with settings
├── Views/
│   ├── BottomDrawer.swift             # Combined drawer implementation
│   ├── FileSystem.swift               # Platform-specific file browsers
│   └── Console/                       # Debug console system
│       ├── DebugConsole.swift         # Main debug console view
│       └── FloatingDebugger.swift     # Floating debug overlay
├── Sources/
│   └── Shared/
│       ├── Extensions/
│       │   └── HapticFeedback.swift   # iOS haptic feedback
│       └── PlatformColor.swift        # Cross-platform color definitions
└── Assets.xcassets/
    └── (App icons and resources)
```

### Key Components Status
- **BottomDrawer.swift**: ✅ Complete - Cross-platform drawer with animations
- **FileSystem.swift**: ✅ Complete - Platform-specific file browsers
- **DebugConsole.swift**: ✅ Complete - Integrated debug console with filtering
- **FloatingDebugger.swift**: ✅ Complete - Optional floating debug overlay
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
    
    var icon: String {
        switch self {
        case .filesystem: return "folder.fill"
        case .debugger: return "terminal.fill"
        case .search: return "magnifyingglass"
        case .inspector: return "info.circle.fill"
        }
    }
}
```

### Debug Console System

#### Message Types and Structure
```swift
enum DebugMessageType {
    case success, error, warning, info, debug, system
    
    var color: Color // Color-coded messages
    var icon: String // SF Symbol for each type
}

struct DebugMessage: Identifiable {
    let id = UUID()
    let timestamp: Date
    let message: String
    let type: DebugMessageType
    let source: String?  // Optional source identifier
}
```

#### Console Features
- **Real-time logging**: Messages appear with timestamps
- **Type filtering**: Filter by error, warning, info, debug
- **Search functionality**: Search through message content and sources
- **Auto-scroll**: Option to automatically scroll to latest messages
- **Clear function**: Clear all messages from console
- **Color-coded messages**: Visual distinction by message type
- **Cross-platform colors**: Adaptive colors for macOS/iOS

#### Floating Debugger
- **Draggable overlay**: Can be positioned anywhere on screen
- **Collapsed/Expanded states**: Minimize to save space
- **Quick filters**: Fast access to error/warning filters
- **Independent from drawer**: Can be used alongside or separately

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

### Search and Inspector Panels

#### Search Panel
- **Search field** with clear button
- **Scope filters**: All Files, Assets, Scripts, Scenes
- **Empty state** with guidance
- **Future**: Will implement actual file search

#### Inspector Panel
- **File properties display**: Name, size, modified date, type
- **Empty state** when no file selected
- **Property grid layout** for easy scanning
- **Future**: Will show actual file metadata

---

## 🎨 UI/UX Architecture

### Layout Philosophy
**Godot-inspired bottom drawer** instead of traditional sidebar:
- Main content area takes full screen
- File browser slides up from bottom
- Tabs for different file views
- Expandable/collapsible with smooth animations
- Debug console integrated as a tab

### Platform Adaptations

#### macOS
- Window-based app with unified toolbar
- Fixed drawer height with drag resize
- Glass material backgrounds
- Hover effects and cursor changes
- Menu bar integration with keyboard shortcuts
- `.help()` modifiers for tooltips

#### iOS  
- Navigation-based with toolbar
- Percentage-based drawer sizing
- Haptic feedback on interactions
- Touch-optimized gestures
- Share sheet integration
- Horizontal scroll for filter buttons

### Visual Design
- **Glass morphism effects** with material backgrounds
- **Smooth spring animations** for all transitions
- **Color-coded elements** for quick identification
- **Consistent spacing** using `GlassConstants`
- **Platform-appropriate styling** with cross-platform color helpers
- **SF Symbols throughout** (no emoji)

### Cross-Platform Color System
```swift
// Custom color extensions for platform compatibility
static var systemBackground: Color
static var secondarySystemBackground: Color  
static var tertiarySystemBackground: Color
static var tertiaryBackground: Color
```

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

### Debug Console Flow
1. User switches to Debug tab in bottom drawer
2. Console shows real-time messages with timestamps
3. User can filter by message type (errors, warnings, etc.)
4. Search functionality for finding specific messages
5. Auto-scroll keeps latest messages visible
6. Clear button removes all messages

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
    ├── thumbnails/        # Generated previews
    └── debug.log          # Debug console persistence
```

---

## 🚧 Current Development State

### ✅ What's Working
- Cross-platform bottom drawer with animations
- Platform-specific file system browsing
- Color-coded file type organization
- Expandable tree structure navigation
- Tab system with four panels
- **Debug console with filtering and search**
- **Floating debugger overlay option**
- **Cross-platform color system**
- Welcome screens with drawer integration
- Settings system (macOS) with preferences

### ⚠️ Known Issues
- **Document package system**: Not yet implemented
- **iCloud synchronization**: Placeholder only
- **File operations**: Limited to display/navigation
- **Search functionality**: UI complete, needs implementation
- **Inspector functionality**: UI complete, needs file metadata
- **Debug persistence**: Messages don't persist between sessions

### 🔄 In Progress
- Document package implementation
- Real file system operations
- iCloud Drive integration
- File import/export workflows
- Search and indexing system
- Debug console persistence

---

## 🛠️ Development Guidelines

### Code Patterns
- Use `#if os(macOS)` for platform-specific code
- Material backgrounds with glass effects
- Spring animations for smooth transitions
- Color-coded elements with SF Symbols
- Consistent spacing using `GlassConstants`
- Cross-platform color helpers for system colors

### Adding Features
1. **New file types**: Add to `FileSystemItem.FileSystemType` enum
2. **New drawer tabs**: Add to `DrawerTab` enum and implement panel
3. **Platform features**: Use conditional compilation
4. **File operations**: Implement in platform-specific panels
5. **Debug messages**: Use `DebugMessage` and `DebugMessageType`

### Debug Console Integration
```swift
// Future integration with actual logging
DebugConsoleView.addMessage(
    "File loaded successfully",
    type: .success,
    source: "FileSystem"
)
```

### Integration Points
- **OrchardBridge**: Will coordinate with other modules
- **RealityStudio**: Will receive file selection events
- **DarwinSyntax**: Will handle script file editing
- **iCloud Drive**: For cross-device synchronization
- **Debug System**: Will connect to engine logging

---

## 🔍 Technical Challenges

### Current Blockers
1. **Document package implementation**: Need NSDocument/UIDocument integration
2. **iCloud synchronization**: Conflict resolution and status monitoring
3. **File operations**: Move, copy, rename, delete functionality
4. **Search indexing**: Content-based search across project files
5. **Debug persistence**: Saving and loading debug logs

### Architecture Decisions
- **Document packages** over virtual filesystem (Apple-native)
- **Bottom drawer** over sidebar (Godot-inspired)
- **Platform-specific implementations** over lowest-common-denominator
- **Material design** over flat design (modern Apple aesthetic)
- **Integrated debug console** over separate window
- **SF Symbols** over custom icons or emoji

---

## 🔗 Module Integration

### Standalone Capabilities
- Project file organization
- Cross-platform file browsing
- Debug message display
- File search interface
- Property inspection
- iCloud synchronization

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
case .debugMessage(DebugMessage)
case .searchRequest(SearchQuery)
```

---

## 📝 Common Development Tasks

### Testing the Current Implementation
1. Run app on macOS and iOS
2. Click "Toggle Files" or "Files" button
3. Verify drawer slides up smoothly
4. Test tab switching between filesystem/debug/search/inspector
5. Verify file tree expansion/collapse
6. Test debug console filtering and search
7. Test drag gestures for drawer resizing
8. Verify cross-platform color consistency

### Debugging Issues
- **Drawer not appearing**: Check `isVisible` binding
- **Animation problems**: Verify spring animation values
- **Platform differences**: Test on both macOS and iOS
- **File tree issues**: Check `expandedFolders` state management
- **Debug console width**: Verify padding and frame constraints
- **Color issues**: Use cross-platform color helpers

### Adding New Debug Features
1. Define new message types in `DebugMessageType`
2. Add appropriate SF Symbol icons
3. Update filter buttons if needed
4. Connect to actual logging sources
5. Consider persistence requirements

---

*This document reflects the actual current state as of January 2025. The debug console system is now fully integrated into the bottom drawer. The focus remains on the bottom drawer interface and platform-specific file browsing, with document packages and iCloud sync as the next major development phase.*