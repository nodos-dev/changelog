# Changelogs

## Nodos Broadcast Bundle

### (DRAFT) From 1.2 to 1.3

**Nodos Engine:**

* Nodos is now able to run on Linux.
* Command line parameter for loading a graph at startup for Launcher.
* Fix potential crashes or hangs when a process node's application is closed/killed or hanged.
* Optimize cycle validation during graph compilation for scheduling to reduce long wait times when editing large graphs.
* Optimize cycle validation when creating pin connections.
* Fix nodes with orphan pins assigned to Idle Runner forever during scheduling.
* Fix crash during engine restart when graph contained a Process Node.
* Fix crash on function node calls after a module reload.
* Add ability to edit & save plugin defined (class-named) graph nodes.
* Fix leaking string buffer resources when a module is unloaded.
* Fix possible race condition when accessing pin data from non-runner threads.
* Fix editor and app service reconnection issues.
* Fix patch number not respected during module dependency resolution.
* Fix pin data invalidated twice during module unload.
* Fixed crash at unloading/loading types of portal pins of class-named graphs.
* Added support for UTF-8 file paths.
* Fixed an issue with path restart coming from an input when multiple inputs are present.
* Added `--disable-beacon` and `--beacon-ip-address` command line arguments to disable beacon broadcasting and set the beacon broadcast IP address mask.
* Fixed an issue with loading/creating class-named graphs inside of class-named graphs.
* Portal pins created on other portal pins now inherit their information from the source portal instead of the real source pin.
* Fixed not removing unloaded nodes from the node list.
* Fixed converting collapsed graphs with a pin to portal pins. This will still be active for pre-1.2 graphs.
* Nodes with unknown typed pins will not be activated and will stay orphan.
* Fixed an issue with template resolving of class-named graphs.
* More descriptive error logging when scanning modules.
* Fix importing a graph inside another graph.
* Removed empty fields from exported graph files.
* Changed execution timing logic.
* Fixed a crash on compilation of long graphs by increasing the stack size.
* Added an exe typed pin to Trigger node.
* Send Undo/Redo stack on new editor connections.
* Reject connections that might result in illegal execution flow, including connecting thread/scheduled nodes to function nodes and connecting a thread node to multiple scheduled nodes and vice versa.
* Create in exe pins on non-event nodes with only out exe pins.
* Added scheduling\_type field to Node fbs, including Regular, Function, and Event.
* Added file\_picker\_type to Visualizer fbs, including OPEN and SAVE.
* Added UTF-8 support for file paths.
* Added DeduplicatedPinGroup metadata to pins.
* Added PinPropertyPaneOrder metadata to pins.
* Added CategoryPropertyPaneOrder metadata to nodes.
* Deprecated has\_responsive\_module of Node fbs; added inactivity\_reasons instead.

**Editor:**

* Recently opened graphs in pulldown menu.
* Drag-drop nodes from Modules Pane to graph.
* Aliases for nodes in the context menu (e.g., png redirects to ReadImage).
* Frame profiler.
* Engines Pane now has detailed info on each Nodos instance in the local area.
* Actions now return result descriptions.
* Action history pane.
* Fix unable to change portal pin's display name.
* De-duplicate outer connections when collapsing nodes into a graph.
* Fix engine module status text stuck at "unloading".
* Revisited look and feel for Engines Pane.
* Show module status messages at the top.
* Apps pane revamped.
* Fix crash when Nodos engine restarted.
* Modules Pane: Fix no other version listed when currently selected version depends on an incompatible API version.
* Fixed open or closed panes are not saved.
* Limit max node width & add an option to configure.
* NEW Native file dialogs for Open/Save graph actions.
* NEW Variables pane for Set/Get variable nodes.
* Fixed crash when a log larger than 4096 characters received.

**SDK:**

* **Breaking Changes:**
    * New major versions for plugin, subsystem, and process SDKs (37, 11, and 18, respectively).
    * Removed deprecated OnNodeUpdated and renamed OnPartialNodeUpdated to OnNodeUpdated.
    * Removed AsyncRequestSubsystem.
    * Removed CopyTo from nosNodeFunctions; use ExecuteNode instead.
    * Removed OnOrphanPinRemoved.
    * Removed CanCreateNode.
    * Renamed nos.fb.Void to nos.Generic.
    * Renamed GetCurrentItemId API to GetCurrentRunnerItemId.
    * Node definition & plugin configuration file structures are redesigned.
    * TemplateParameters struct now has a name field.
    * nos.fb.Track and nos.fb.LensDistortion are removed from builtin types and are now defined by nos.track plugin.
    * nos.fb.CaptureData and nos.fb.CaptureDataArray types are deleted from builtin types.
    * `allows_cyclic` field in node definitions is removed.
    * In node definition files for class-named graphs, pin values are now loaded from source pins instead of outer graph portal pin values.
    * `nosFbNode` and similar typedefs are deprecated; use `nosFbNodePtr` and similar instead.
    * SendCustomEditorMessage API is changed to SendEditorMessage and expects a `nosSendEditorMessageParams`.
    * Buffer returned by `nosEngine.GetDefaultValueOfType` should now be freed with `nosEngine.FreeBuffer`.
* **New:**
    * Implement RequestSubsystemForNode API.
    * Add various methods to node helper base class in C++ SDK.
    * Add MSVC visualizer for nosName.
    * Improve deadlock avoidance for Process nodes.
    * Added EngineBuffer and EngineString C++ helper classes.
    * Added a helper macro for giving execute procedures to function nodes using NOS\_DECLARE\_FUNCTIONS & NOS\_ADD\_FUNCTION.

**Modules:**

* **nos.reflect - 1.6.4.b907 (prev: 1.1.2.b700, 1.3.2.b786, 1.4.3.b839)**
    * Enum arrays are added.
    * Fix: Array nodes crash or malfunction occasionally.
    * SetVariable node now has OnVariableUpdated event.
    * Fix: variables are not restored when only GetVariable nodes are present in the saved graph.
    * Added generic LessThan and GreaterThan nodes.
* **nos.sys.shaderc - 1.1.0.b878 (prev: 1.1.0.b667, 1.1.0.b777, 1.1.0.b784)**
* **nos.sys.vulkan - 6.3.3.b644 (prev: 5.22.0.b551, 5.23.1.b554, 5.24.0.b573, 5.25.2.b588, 6.1.0.b595, 6.1.2.b608)**
    * Shader nodes now accept array types.
    * Fix: crash on device lost.
    * Added missing image formats for swapchain.
    * Fix possible race conditions when accessing swapchain.
    * Fix unable to load plugins with runtime-compiled shaders without glslc on PATH.
    * Added GPU device selection & validation layer settings.
    * **Breaking Changes:**
        * CreateResource now requires a mandatory tag.
        * SetResourceTag is removed.
        * Begin functions now require a nosCmdBeginParams and removed Begin2.
        * RegisterShader replaced with RegisterShader2.
    * **New:**
        * Added C++ helpers for managing resources & commands.
        * Added black stock texture option for plugins.
        * Added option to use dedicated transfer queues for copy operations over PCI.
* **nos.sys.settings - 0.3.0.b804 (NEW)**
* **nos.filters - 1.5.2.b880 (prev: 1.2.0.b696, 1.2.1.b786, 1.4.0.b845)**
* **nos.math - 1.9.3.b907 (prev: 1.4.0.b698, 1.7.0.b777, 1.8.1.b814)**
    * Removed Add\_f32 and similar nodes & added migrations to nos.reflect plugin for Arithmetic nodes.
* **nos.utilities - 3.9.6.b962 (prev: 2.12.1.b704, 2.13.0.b713, 3.1.3.b786, 3.3.3.b845, 3.8.2.b887, 3.9.1.b902)**
    * NEW String utility nodes: Regex, Json2Pin, Pin2Json.
    * Add option to use host-cached buffers in UploadBufferProvider.
    * Added SwitchTrigger node.
    * Added ConditionalTrigger node.
    * Added TriggerOnAnyInput node.
    * Added TextureMonitor node.
    * Fixed ReadImage trying to load network file info in Nodos thread.
* **nos.mediaio - 2.5.0.b641 (prev: 2.0.0.b592, 2.1.0.b602, 2.3.2.b636, 2.4.0.b638)**
    * NEW Add RGBA to RGB24 conversion node.
    * Added GetLumaCoeffs graph.
* **nos.sys.mediaio - 0.4.0.b641 (prev: 0.1.0.b598, 0.2.0.b623, 0.3.0.b638)**
* **nos.webcam - 1.3.2.b646 (prev: 1.1.0.b575, 1.2.0.b583, 1.2.1.b624, 1.3.0.b637, 1.3.1.b641)**
    * NEW Add webcam output node.
    * Fixed black screen when two same channels are opened.
* **nos.sys.animation - 1.5.1.b902 (prev: 1.2.0.b667, 1.2.0.b777, 1.4.0.b823)**
* **nos.animation - 0.3.2.b902 (prev: 0.2.0.b823)**
* **nos.webrtc - 1.2.0.b878 (prev: 1.1.0.b687, 1.1.1.b786, 1.1.3.b848)**
* **nos.str - 0.3.0.b878 (NEW)**
    * New string nodes: Tokenizer, Regex...
* **nos.aja - 2.6.1.b731 (prev: 2.2.1.b634, 2.2.2.b671, 2.2.3.b673, 2.4.0.b683, 2.5.2.b700)**
    * Added a new pin to acquire/release device for exclusive use on the system.
    * Device pin is now using nos.sys.device for better device management.
* **nos.device.decklink - 0.1.0.b4**
* **nos.decklink - 1.1.2.b73 (prev: 0.1.0.b4, 0.1.1.b10, 0.3.7.b59, 1.0.0.b65)**
    * NEW Decklink SDI support.
    * Add new device support: DeckLink Studio 4K, DeckLink 4K Pro.
    * Show drop counts on node.
    * Now works on Linux.
    * Fix crashes and hangs when device profile is changed.
    * Use host-cached & aligned buffers in input to increase DMA performance.
    * Show reference lock state in node status.
    * Experimental: Interlaced support.
    * Device temperature & PCIe bus state watch-logs.
    * Fix crash on device profile change.
* **nos.sys.decklink - 2.1.0.b70 (prev: 1.4.2.b59, 2.0.0.b65)**
* **nos.track - 1.9.2.b957 (prev: 1.6.0.b696, 1.6.0.b713, 1.6.0.b777, 1.8.0.b823, 1.9.0.b878, 1.9.1.b902)**
    * Make Track output pin orphan when UDP data is not available.
    * Added Track, CaptureData, and LensDistortion data types.
    * Moved AddTrack from nos.math to nos.track.
* **nos.display - 0.3.0.b26 (NEW)**
    * NEW DisplayOut Node using HDMI ports.
* **nos.sys.device - 0.3.3.b15 (NEW)**
* **nos.sys.mediaio - 0.4.0.b641 (prev: 0.3.0.b638)**
* **nos.webcam - 1.3.2.b646 (prev: 1.3.1.b641)**
* **nos.sys.animation - 1.5.1.b902 (prev: 1.5.0.b878)**
* **nos.animation - 0.3.2.b902 (prev: 0.3.0.b878)**