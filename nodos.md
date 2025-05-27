# Nodos Broadcast Bundle Changelog

Changelogs from version 1.2.6 to 1.3.0.

## Nodos Engine

* **New:**
    * Nodos is now able to run on Linux
    * Frame Profiler
    * A command line parameter for loading a graph at startup for Launcher
    * `--disable-beacon` and `--beacon-ip-address` command line arguments
    * Ability to edit & save plugin defined (class-named) graph nodes
    * Trigger node now has `exe` typed input pin
    * `scheduling_type` field to Node fbs (Regular, Function, Event)
    *  `file_picker_type` to Visualizer fbs (OPEN, SAVE)
    * Support for UTF-8 file paths
* **Changed:**
    * Portal pins created on other portal pins now inherit their information from the source portal
    * Nodes with unknown typed pins will not be activated and will stay orphan
    * When saving a graph, empty fields are removed to reduce graph size
    * Improved execution timing logic
    * Rejecting connections that might result in illegal execution flow
    * Deprecated `has_responsive_module` of Node fbs; added `inactivity_reasons` instead
    * Provided more descriptive error logging when scanning modules
    * Updates from nodes are now accepted in live mode
* **Fixed:**
    * Modules are now able to be reliably unloaded/loaded while graph is running
    * Increase stability for single-input to multi-out scenarios
    * Crash on compilation of long graphs by increasing the stack size
    * Potential crashes or hangs when a process node's application is closed/killed or hanged
    * Crash during engine restart when a graph contained a Process Node
    * Crash on function node calls after a module reload
    * Importing a graph inside another graph
    * Nodes with orphan pins being assigned to Idle Runner forever during scheduling
    * Leaking string buffer resources when a module is unloaded
    * Possible race condition when accessing pin data from non-runner threads
    * Editor and app service reconnection issues
    * Patch number not being respected during module dependency resolution
    * Pin data being invalidated twice during module unload
    * Crash when unloading/loading types of portal pins of class-named graphs
    * Issue with path restart coming from an input when multiple inputs are present
    * Issue with loading/creating class-named graphs inside of class-named graphs
    * Not removing unloaded nodes from the node list
    * Converting collapsed graphs with a pin to portal pins (for pre-1.2 graphs)
    * Template resolving of class-named graphs
    * Optimized cycle validation during graph compilation for scheduling
    * Optimized cycle validation when creating pin connections
    * Undo/Redo stack not being sent to new editor connections

## Editor

* **New:**
    * Action History pane
    * Variables pane for Set/Get variable nodes
    * Native file dialogs for Open/Save graph actions
    * Option to sort properties in Property Pane in editors.
    * Portal On Parent: A single property on parent graph controls multiple child node properties
    * Option to sort categories in Property Pane in editors.
    * Option to add new categories in Property Pane in editors.
    * "Recently opened graphs" pulldown menu
    * Drag-drop of nodes from the Modules Pane to the graph
    * Aliases for nodes in the context menu (e.g., `png` redirects to `ReadImage`)
* **Changed:**
    * Engines Pane now has detailed info and a revisited look and feel
    * Actions now return result descriptions
    * De-duplicated outer connections when collapsing nodes into a graph
    * Module status messages are now shown at the top
    * Revamped the Apps pane
    * Limited max node width & added a configuration option
* **Fixed:**
    * Unable to change a portal pin's display name
    * Engine module status text getting stuck at "unloading"
    * Crash when the Nodos engine restarted
    * No other version was listed in Modules Pane if the current version depended on an incompatible API version
    * Open or closed panes not being saved
    * Crash when a log larger than 4096 characters was received
    * Unable to navigate in node graph during live mode
    * Incorrectly showing a property as modified/unmodified in Property Pane sometimes

## SDK

* **New:**
    * `RequestSubsystemForNode` API for managing dependencies at node-level
    * Various methods to the node helper base class in C++ SDK
    * MSVC visualizer for `nosName`.
    * `EngineBuffer` and `EngineString` C++ helper classes for managing Nodos allocated buffers
    * Helper macro (`NOS_DECLARE_FUNCTIONS` & `NOS_ADD_FUNCTION`) for function nodes
* **Removed (Breaking Changes):**
    * Removed deprecated `OnNodeUpdated` and renamed `OnPartialNodeUpdated` to `OnNodeUpdated`
    * Removed `AsyncRequestSubsystem`
    * Removed `CopyTo` from `nosNodeFunctions`; use `ExecuteNode` instead
    * Removed `OnOrphanPinRemoved`
    * Removed `CanCreateNode`.
    * Renamed `nos.fb.Void` to `nos.Generic`
    * Renamed `GetCurrentItemId` API to `GetCurrentRunnerItemId`
* **Changed (Breaking Changes):**
    * New major versions for plugin (37), subsystem (11), and process (18) SDKs
        - Plugin SDK version updated from 36.1.1 to 37.4.1
        - Subsystem SDK version updated from 10.1.1 to 11.5.0
        - Process SDK version updated from 15.0.0 to 18.0.0
    * Redesigned Node definition & plugin configuration file structures
    * `TemplateParameters` struct now has a `name` field
    * `nos.fb.Track` and `nos.fb.LensDistortion` were removed from built-in types (now in `nos.track`)
    * `nos.fb.CaptureData` and `nos.fb.CaptureDataArray` types were deleted from built-in types
    * `allows_cyclic` field in node definitions was removed
    * Changed how pin values are loaded in node definition files for class-named graphs
    * Deprecated `nosFbNode` and similar typedefs; use `nosFbNodePtr` instead
    * Changed `SendCustomEditorMessage` API to `SendEditorMessage`
    * Buffer returned by `nosEngine.GetDefaultValueOfType` now needs freeing
    * Improved deadlock avoidance for Process nodes
* **Fixed**
    * Enums were not included in 

## Modules

* **New Modules**
    * **nos.animation:** Nodes for animating values on node graph: `Animate`, `AnimationCurve`, `Interpolate`
        - Support for custom interpolators
    * **nos.sys.variables:** Nodes for managing named variables on node graph: `Get`, `Set`
    * **nos.decklink:** Support for Blackmagic Design Decklink SDI I/O
    * **nos.sys.decklink:** Subsystem for managing DeckLink devices
    * **nos.sys.device:** For managing devices on the system
    * **nos.display:** `DisplayOut` Node using HDMI ports
    * **nos.sys.settings:** Subsystem for helping plugins persist their settings
    * **nos.str:** String nodes like `Tokenizer`, `Regex`, `Json2Pin`, `Pin2Json`
    * **nos.rive (Experimental):** For rendering Rive artboards
* **nos.aja:**
    * **New:** A new pin manage acquire/release state of the device for exclusive use
    * **Changed:** Device pin now uses `nos.sys.device` for better device management
* **nos.math:**
    * **Changed:** Removed `Add_f32` and similar nodes & added migrations to `nos.reflect` for Arithmetic nodes
* **nos.mediaio:**
    * **New**
        - `RGBA` to `RGB24` conversion node
        - `GetLumaCoeffs` graph.
* **nos.reflect:**
    * **New**
        - Enum arrays
        - Generic `LessThan` and `GreaterThan` nodes
    * **Changed:** `SetVariable` node now has `OnVariableUpdated` event.
    * **Fixed**
        - Array nodes crash or malfunction occasionally
        - Variables are not restored when only `GetVariable` nodes are present
* **nos.sys.vulkan:**
    * **New**
        - Missing image formats for swapchain
        - GPU device selection & validation layer settings
        - C++ helpers for managing resources & commands
        - Black stock texture option for plugins
        - Option to use dedicated transfer queues for copy operations over PCI
    * **Changed**
        - Shader nodes now accept array types
        - `CreateResource` now requires a mandatory tag
        - `SetResourceTag` is removed
        - `Begin` functions now require a `nosCmdBeginParams` and `Begin2` was removed
        - `RegisterShader` replaced with `RegisterShader2`
    * **Fixed**
        - Crash on device lost
        - Variables are not restored when only `GetVariable` nodes are present
        - Possible race conditions when accessing swapchain
        - Unable to load plugins with runtime-compiled shaders without `glslc` on PATH
* **nos.track:**
    * **New:** `Track`, `CaptureData`, and `LensDistortion` data types (moved from built-in)
    * **Changed**
        - Track output pin becomes orphan when UDP data is not available
        - Moved `AddTrack` from `nos.math` to `nos.track`
* **nos.utilities:**
    * **New**
        - New String utility nodes: `Regex`, `Json2Pin`, `Pin2Json`
        - Option to use host-cached buffers in `UploadBufferProvider`
        - `SwitchTrigger` node
        - `ConditionalTrigger` node
        - `TriggerOnAnyInput` node
        - `TextureMonitor` node
    * **Fixed:** `ReadImage` trying to load network file info in Nodos thread
* **nos.webcam:**
    * **New:** Webcam output node
    * **Fixed:** Black screen when two same channels are opened

* **nos.webrtc:**
    * **New:** HTTPS support