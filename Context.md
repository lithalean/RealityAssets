# RealityAssets — AI Context Document

**Date**: January 2025  
**Status**: Active Development - Real File System Implementation Complete  
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
- **Real file system browsing with actual file operations**

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
│   ├── FileSystem/                    # Real file system implementation
│   │   ├── FileSystemModel.swift      # Core file system data model
│   │   ├── FileSystemView.swift       # Main file browser view
│   │   └── FilePermissionsHelper.swift # iOS file access helpers
│   └── Console/                       # Debug console system
│       ├── DebugConsole.swift         # Main debug console view
│       └── FloatingDebugger.swift     # Floating debug overlay
├── Sources/
│   └── Shared/
│       ├── Extensions/
│       │   └── HapticFeedback.swift   # iOS haptic feedback
│       ├── PlatformColor.swift        # Cross-platform color definitions
│       └── GlassConstants.swift       # UI spacing and style constants
└── Assets.xcassets/
    └── (App icons and resources)
```

### Key Components Status
- **BottomDrawer.swift**: ✅ Complete - Cross-platform drawer with animations
- **FileSystemModel.swift**: ✅ Complete - Real file system data model with UTI support
- **FileSystemView.swift**: ✅ Complete - Functional file browser with sorting/filtering
- **FilePermissionsHelper.swift**: ✅ Complete - iOS document picker and bookmarks
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

### Real File System Implementation

#### Core Model Structure
```swift
struct FileSystemItem: Identifiable {
    let id = UUID()
    let url: URL
    let name: String
    let isDirectory: Bool
    let fileType: FileType
    let size: Int64?
    let modificationDate: Date?
    let isHidden: Bool
    var children: [FileSystemItem]?
}

@MainActor
class FileSystemViewModel: ObservableObject {
    @Published var rootItems: [FileSystemItem] = []
    @Published var expandedFolders: Set<URL> = []
    @Published var selectedItem: FileSystemItem?
    // Manages file system state and operations
}
```

#### File Type Classification
- **Smart detection**: Uses UTI (Uniform Type Identifiers) and file extensions
- **Categorized types**: Scripts, images, models, audio, documents, archives
- **Color-coded icons**: Each file type has unique color and SF Symbol
- **Directory detection**: Special handling for Assets, Scenes, Scripts folders

#### Platform-Specific Features

**macOS (Full Access)**:
- Browse entire file system
- System shortcuts: Home, Desktop, Documents, Downloads, Applications
- Finder integration: "Reveal in Finder" context menu
- Terminal integration: "Open in Terminal" for directories
- Direct file opening with default applications
- No permission restrictions

**iOS (Sandboxed)**:
- Limited to app's Documents and iCloud containers
- Document picker for importing external files/folders
- Security-scoped bookmarks for persistent access
- Files app integration
- Orchard Projects folder auto-creation

#### File Browser Features
- **Real-time navigation**: Click to expand/collapse directories
- **Sorting options**: Name (↑↓), Date (↑↓), Type (↑)
- **Hidden files toggle**: Show/hide system files
- **File metadata display**: Size and modification date
- **Context menus**: Platform-appropriate actions
- **Double-click to open**: Files open in default apps (macOS)
- **Hover effects**: Visual feedback on macOS
- **Selection state**: Highlighted selected items

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

### Search and Inspector Panels

#### Search Panel
- **Search field** with clear button
- **Scope filters**: All Files, Assets, Scripts, Scenes
- **Empty state** with guidance
- **Future**: Will connect to real file search

#### Inspector Panel
- **File properties display**: Name, size, modified date, type
- **Empty state** when no file selected
- **Property grid layout** for easy scanning
- **Future**: Will show selected file's actual metadata

---

## 🎨 UI/UX Architecture

### Layout Philosophy
**Godot-inspired bottom drawer** instead of traditional sidebar:
- Main content area takes full screen
- File browser slides up from bottom
- Tabs for different file views
- Expandable/collapsible with smooth animations
- Each tab manages its own ScrollView when needed

### Platform Adaptations

#### macOS
- Window-based app with unified toolbar
- Fixed drawer height with drag resize
- Glass material backgrounds
- Hover effects and cursor changes
- Menu bar integration with keyboard shortcuts
- `.help()` modifiers for tooltips
- Context menus with right-click

#### iOS  
- Navigation-based with toolbar
- Percentage-based drawer sizing
- Haptic feedback on interactions
- Touch-optimized gestures
- Share sheet integration
- Document picker for file imports
- Horizontal scroll for filter buttons when needed

### Visual Design
- **Glass morphism effects** with material backgrounds
- **Smooth spring animations** for all transitions
- **Color-coded elements** for quick identification
- **Consistent spacing** using `GlassConstants`
- **Platform-appropriate styling** with cross-platform color helpers
- **SF Symbols throughout** (no emoji)

### Cross-Platform Systems

#### Color System
```swift
// Custom color extensions for platform compatibility
static var systemBackground: Color
static var secondarySystemBackground: Color  
static var tertiarySystemBackground: Color
static var tertiaryBackground: Color
```

#### Constants System (GlassConstants)
- **Spacing**: tight (4), small (8), medium (16), large (24)
- **Radius**: small (6), medium (12), large (16), xl (20), xxl (24)
- **Materials**: light (0.1), medium (0.2), heavy (0.3) opacity
- **Animations**: quick (0.2), standard (0.3), slow (0.5) seconds

---

## 📱 Current User Experience

### Welcome Screen
**macOS**: Desktop-style with toolbar and large buttons
**iOS**: Mobile-friendly with navigation and stacked buttons

### File Management Flow
1. User opens app to welcome screen
2. Clicks "Toggle Files" or "Files" button
3. Bottom drawer slides up with real file browser
4. User sees platform-appropriate root locations
5. Navigate by clicking folders to expand/collapse
6. View file details (size, date) inline
7. Use context menu for file operations
8. Sort/filter with header controls

### Debug Console Flow
1. User switches to Debug tab in bottom drawer
2. Console shows real-time messages with timestamps
3. User can filter by message type (errors, warnings, etc.)
4. Search functionality for finding specific messages
5. Auto-scroll keeps latest messages visible
6. Clear button removes all messages

### iOS File Import Flow
1. User taps import button in file browser
2. System document picker appears
3. User selects file or folder
4. App creates security-scoped bookmark
5. File/folder appears in file browser
6. Access persists across app launches

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
- **Real file system browsing** with actual files and directories
- **Platform-specific file access** (full on macOS, sandboxed on iOS)
- **File type detection** with UTI and extension support
- **Smart folder recognition** (Assets, Scripts, Scenes, etc.)
- **File metadata display** (size, modification date)
- **Sorting and filtering** options
- **Context menus** with platform-appropriate actions
- **iOS document picker** integration
- **Security-scoped bookmarks** on iOS
- Cross-platform bottom drawer with animations
- Color-coded file type organization
- Expandable tree structure navigation
- Tab system with four functional panels
- Debug console with filtering and search
- Floating debugger overlay option
- Cross-platform color and constants system
- Welcome screens with drawer integration
- Settings system (macOS) with preferences

### ⚠️ Known Issues
- **Document package system**: Not yet implemented
- **iCloud synchronization**: Detection only, no sync
- **File operations**: No rename, move, delete, copy yet
- **Search functionality**: UI complete, needs file content search
- **Inspector functionality**: UI complete, needs deeper file metadata
- **Debug persistence**: Messages don't persist between sessions
- **File previews**: No thumbnail generation yet

### 🔄 Next Steps
1. **File Operations**: Implement rename, move, delete, copy
2. **Search Integration**: Connect search UI to actual file search
3. **Inspector Integration**: Show detailed file info in Inspector tab
4. **Document Packages**: Create/open `.orchard` project packages
5. **iCloud Sync**: Full synchronization support
6. **File Previews**: Generate thumbnails for images/models
7. **Project Templates**: New project creation workflow

---

## 🛠️ Development Guidelines

### Code Patterns
- Use `#if os(macOS)` for platform-specific code
- Material backgrounds with glass effects
- Spring animations for smooth transitions
- Color-coded elements with SF Symbols
- Consistent spacing using `GlassConstants`
- Cross-platform color helpers for system colors
- Lazy loading for directory contents
- Security-scoped resources on iOS

### Adding Features
1. **New file types**: Add to `FileType` enum with icon and color
2. **New drawer tabs**: Add to `DrawerTab` enum and implement panel
3. **Platform features**: Use conditional compilation
4. **File operations**: Add to `FileSystemViewModel`
5. **Debug messages**: Use `DebugMessage` and `DebugMessageType`
6. **iOS permissions**: Use `FilePermissionsHelper` for external access

### File System Integration
```swift
// Detecting file types
FileType.fromUTI(typeIdentifier, fileName: name)
FileType.fromDirectoryName(name)

// Platform actions
#if os(macOS)
viewModel.revealInFinder(item)
viewModel.openInTerminal(item)
#endif

// File operations
viewModel.toggleFolder(item)
viewModel.selectItem(item)
viewModel.openFile(item)
```

### Integration Points
- **OrchardBridge**: Will coordinate with other modules
- **RealityStudio**: Will receive file selection events
- **DarwinSyntax**: Will handle script file editing
- **iCloud Drive**: For cross-device synchronization
- **Debug System**: Will connect to engine logging

---

## 🔍 Technical Implementation Details

### File System Architecture
1. **Lazy Loading**: Directories load children only when expanded
2. **State Management**: `@Published` properties for reactive UI
3. **URL-based**: Uses Foundation's URL for all file references
4. **Bookmark Persistence**: iOS saves security-scoped bookmarks
5. **Type Detection**: UTI-based with fallback to extensions

### Performance Considerations
- **Lazy LazyVStack**: Only renders visible file items
- **Debounced Updates**: Prevents excessive redraws
- **Cached Metadata**: File info stored in model
- **Smart Diffing**: SwiftUI handles incremental updates

### Security Model
- **macOS**: Full filesystem access (user permissions apply)
- **iOS**: Sandboxed with explicit user grants via document picker
- **Bookmarks**: Persistent access to user-selected locations
- **No Network Access**: Local files only (iCloud pending)

---

## 📝 Common Development Tasks

### Testing the File Browser
1. Run app on both macOS and iOS
2. Navigate to FileSystem tab
3. Verify platform-appropriate root locations
4. Test folder expansion/collapse
5. Check file metadata display
6. Test sorting options
7. Toggle hidden files
8. Test context menu actions (macOS)
9. Test document picker (iOS)

### Adding New File Types
1. Add case to `FileType` enum
2. Define icon (SF Symbol) and color
3. Add detection logic to `fromUTI` or extension check
4. Test with sample files

### Debugging File Access Issues
- **iOS Permission Denied**: Check security-scoped resource access
- **Missing Files**: Verify hidden files toggle
- **Slow Loading**: Check for synchronous operations
- **Wrong Icons**: Verify UTI detection logic

---

*This document reflects the actual current state as of January 2025. The file system is now fully functional with real file browsing on both platforms. The next major phase is implementing file operations and document package support.*