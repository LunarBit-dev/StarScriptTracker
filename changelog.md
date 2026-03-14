# Changelog

All notable changes to StarScript will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.7.2] - 2026-03-14

### Removed
- **Blueprint Editor (shelved)** - Removed blueprint editor commands, networking, and runtime components from the active build.
- Removed `/story blueprint ...` command tree and related server/client packet wiring.

### Changed
- Documentation now reflects builder and JSON workflows as the active authoring paths.
- Node runtime now supports `show_gui: false` for headless/system nodes.
- Start-node trigger flows can opt into trigger-first startup with `auto_start_trigger: true`.
- Choice action parsing now supports both `actions` and `on_choice` for compatibility.
- Story text interpolation now supports `{var}`, `${var}`, and `{{var}}` placeholders.
- Visual novel text rendering uses clipped text-area drawing and improved wheel handling to reduce overlap/clipping issues.
- Relic authoring/schema expanded with abilities, passives, cooldown/energy, upgrades, synergies, alignment, set/slot metadata, discovery spawn metadata, and procedural modifiers.

### Added
- New no-GUI structure trigger test story: `structure_entry_nogui_test_story.json`.
- Relic ability runtime support (`detect_structure`, `grant_effect`, `trigger_scene`, `teleport_short`, `summon_entity`, `scan_area`, `reveal_lore`).
- Relic inventory/equipment/puzzle trigger hooks (`relic_used`, `relic_in_inventory`, `relic_equipped`, `relic_near_structure`, `relic_channel_complete`, `relic_activate`).
- Optional relic set bonus and multi-relic synergy evaluation.
- Client-side guard to skip opening `StoryNodeScreen` when a node is marked `show_gui: false`.

### Fixed
- Gradle `jar` task property mismatch (`archivesBaseName` -> `archives_base_name`) for modern Gradle compatibility.

## [0.7.1] - 2026-01-09

### Added - Builder Pattern System
- **StoryBuilder** - Fluent API for constructing complete stories programmatically
- **StoryNodeBuilder** - Type-safe node construction with nested GuiStyleBuilder and TextEffectBuilder
- **StoryActionBuilder** - 15+ factory methods for common actions (giveItem, teleport, playSound, etc.)
- **StoryTriggerBuilder** - 18+ factory methods for all trigger types (mobKill, proximity, bossDeath, etc.)
- **StoryChoiceBuilder** - Build player choices with actions, timeouts, and weighted outcomes
- **BuilderExamples.java** - 8 complete example stories demonstrating all builder features

### Added - Integration & Testing
- **Blueprint Editor Integration** - Updated to use builder pattern for story creation
- **ProGuard Optimization** - Build pipeline includes obfuscation and optimization
- **Comprehensive Test Story** - `comprehensive_test_story.json` tests all features
- **Test Script** - Single command to test every feature in the mod

### Changed
- Updated BlueprintEditor to use immutable builders instead of setters
- Fixed BlueprintEditorScreen rendering for Minecraft 1.21.11
- Improved build pipeline with ProGuard integration
- Enhanced JenkinsFile with optimization stage

### Technical
- 2,018 lines of production builder code
- 800+ lines of documentation
- 33 factory methods for quick object creation
- Full support for modifying existing stories with `.from()`
- JSON export capability from builders
- Nested builder support for complex configurations

### Developer Experience
- Compile-time validation of story structure
- Required fields checked at build time
- Type-safe API prevents runtime errors
- Fluent chainable method calls for readability
- Full JavaDoc documentation
- Integration examples for custom mods

### Fixed
- **Network Payload Registration** - Fixed ClassCastException with OpenBlueprintEditorPayload
  - Properly registered blueprint editor payload in PayloadTypeRegistry
  - Added client-side network handler registration
  - Blueprint editor now opens without network errors

## [0.7.0] - 2026-01-07

### Added - Story Features
- **Proximity Triggers** - Location-based story progression with coordinate + radius detection
- **Depth/Altitude Triggers** - Y-level based events (underground/high altitude detection)
- **Time-Based Triggers** - Day/night cycle detection and world day counting
- **Health/Status Triggers** - Health threshold monitoring and potion effect detection
- **Weighted Choices** - Probabilistic branching for dynamic, randomized outcomes
- **Custom Bosses** - Enhanced entities with custom stats, abilities, drops, and boss bars
- **Variable System Enhancements** - Full support for int, bool, and string variables
- Advanced trigger types: `proximity`, `depth_below`, `altitude_above`, `night_only`, `day_only`, `health_below`, `potion_effect`
- `advanced_features_demo.json` test story showcasing all new features
- `comprehensive_feature_test.json` complete feature test suite

### Added - Technical Infrastructure
- Complete error hierarchy (NetworkError, ResourceError, AuthenticationError, AuthorizationError)
- YAML configuration file support (in addition to JSON)
- Configuration file discovery system (4-tier search: env var → cwd → home → system)
- Enhanced logging system with 5 levels (ERROR, WARN, INFO, DEBUG, TRACE)
- Sensitive data redaction in logs (passwords, API keys, tokens)
- Environment variable configuration (LUNARCORE_* prefix)
- VERSION constant exposed for programmatic access
- Comprehensive SECURITY.md policy
- Full LunarCore specification compliance (100%)

### Changed
- Improved trigger matching system for new trigger types
- Enhanced StoryEngine to support weighted outcomes
- Updated StoryChoice to include weighted_outcomes field
- Refactored configuration system for better precedence handling
- Optimized trigger performance with efficient caching

### Fixed
- Mixin compilation error (removed duplicate PlayerDataPersistenceMixin)
- Boss spawning world access issue
- Typewriter effect no longer cuts off last character
- Save system now properly persists and restores state
- Variable system fully functional with proper type handling
- GUI scrolling works correctly with mouse wheel
- Back button navigates properly instead of closing GUI
- Right-click no longer closes story GUI unexpectedly
- Button layout improved for 3+ choices (side-by-side display)

### Security
- Added secrets redaction in logs
- Removed hardcoded test credentials
- Implemented comprehensive input validation for story data
- Added vulnerability disclosure policy
- Environment segregation support

### Performance
- Event-driven trigger system (minimal CPU overhead)
- Position-based triggers only check on player movement
- Efficient state caching per player
- Automatic cleanup on player disconnect
- Hot-reload optimization (<100ms for story files)

## [0.6.8] - 2025-12-20

### Added
- Variable system (int, bool, string variables)
- Call/return functionality for sub-scenes
- Scene effects (transitions, overlays, screenshake)
- Custom GUI styling per node
- Objectives HUD system

### Changed
- Improved GUI rendering pipeline

### Fixed
- Memory leak in trigger cache
- GUI rendering performance issues

## [0.6.7] - 2025-12-15

### Added
- Save/load system with 10 slots
- Trigger system (item, block, world, combat, exploration)
- Story validation on load
- Debug command mode
- Player advancement tracking

### Changed
- Enhanced story state management

### Fixed
- Story state synchronization issues
- Choice button layout for 3+ options

## [0.6.0] - 2025-12-01

### Added
- Initial release
- Story engine with JSON-based stories
- Node-based narrative structure
- Choice system with branching
- Basic GUI with typewriter effect
- Command system (`/story start`, `/story list`, etc.)
- Flag system for story progression
- Basic trigger support

---

## Version Support Policy

- **Current MAJOR version (0.7.x)** - Full support, all updates
- **Previous MAJOR version (0.6.x)** - Security fixes only, 6 months
- **Older versions (< 0.6)** - No support, upgrade required

## Migration Guides

### Migrating to 0.7.0 from 0.6.x

✅ **No breaking changes!** All 0.6.x stories are 100% compatible with 0.7.0.

**New Features (Opt-in):**

**1. Weighted Choices:**
```json
{
  "text": "Search the chest",
  "weighted_outcomes": [
    {"next": "treasure", "weight": 60, "actions": [...]},
    {"next": "nothing", "weight": 30},
    {"next": "trap", "weight": 10}
  ]
}
```

**2. Advanced Triggers:**
```json
{
  "triggers": [
    {"type": "proximity", "x": 100, "y": 64, "z": 200, "radius": 10},
    {"type": "depth_below", "y": 0},
    {"type": "night_only"},
    {"type": "health_below", "threshold": 10.0}
  ]
}
```

**3. Custom Bosses:**
```json
{
  "custom_bosses": [{
    "id": "my_boss",
    "name": "§4§lMy Boss",
    "base_entity": "minecraft:zombie",
    "health_multiplier": 5.0,
    "damage_multiplier": 2.0
  }]
}
```

**4. Configuration:**
```bash
# New environment variables
export LUNARCORE_LOG_LEVEL=DEBUG
export LUNARCORE_DEBUG_MODE=true
```

**All existing story features continue to work exactly as before.**

---

## Statistics

### Version 0.7.0 by the Numbers
- **30+ Features** implemented
- **8 Trigger Categories** with 15+ trigger types
- **6 Action Categories** with 18+ action types
- **7 Error Types** for proper error handling
- **100% LunarCore Compliance**
- **463-line README** (down from 1117, more informative)
- **2 Complete Test Stories** included
- **10 Save Slots** per story
- **<1ms** trigger check overhead per tick
- **<100ms** hot-reload time for stories

---

[Unreleased]: https://github.com/LunarBit-dev/StarScript/compare/v0.7.0...HEAD
[0.7.0]: https://github.com/LunarBit-dev/StarScript/compare/v0.6.8...v0.7.0
[0.6.8]: https://github.com/LunarBit-dev/StarScript/compare/v0.6.7...v0.6.8
[0.6.7]: https://github.com/LunarBit-dev/StarScript/compare/v0.6.0...v0.6.7
[0.6.0]: https://github.com/LunarBit-dev/StarScript/releases/tag/v0.6.0

