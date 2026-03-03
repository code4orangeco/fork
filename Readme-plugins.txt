these are the manifests for fork to make plugins

You can use any manifest keys you want, but only one is actually required by your Fork plugin system. Everything else is optional metadata that you can define for your own loader.

To make this clear and useful, here are:

The required manifest entry your loader already supports

Optional manifest entries you can add (and how they would work)

A complete example manifest

How to extend your loader to support more keys

🔑 Required manifest entry (your system already uses this)
Your Fork plugin loader requires exactly one key:

Fork-Plugin-Class
Points to the main class of the plugin

Must implement your ForkPlugin interface

Without this, the plugin will not load

Example:

Code
Fork-Plugin-Class: com.example.myplugin.MyPlugin
🧩 Optional manifest entries you can support
These are not built-in to your system yet, but they are common in plugin ecosystems and easy to add.

Fork-Plugin-Name
Human-readable name.

Code
Fork-Plugin-Name: MyPlugin
Fork-Plugin-Version
Version string.

Code
Fork-Plugin-Version: 1.0.0
Fork-Plugin-Author
Plugin author.

Code
Fork-Plugin-Author: orangeco
Fork-Plugin-Description
Short description.

Code
Fork-Plugin-Description: Adds custom features to the Fork server.
Fork-Plugin-Depends
Comma-separated list of other Fork plugins that must load first.

Code
Fork-Plugin-Depends: CoreLib, Utils
Fork-Plugin-SoftDepends
Optional dependencies.

Code
Fork-Plugin-SoftDepends: ChatAPI
Fork-Plugin-LoadBefore
Plugins that should load after this one.

Code
Fork-Plugin-LoadBefore: WorldTweaks
Fork-Plugin-API-Version
If you ever version your ForkPlugin interface.

Code
Fork-Plugin-API-Version: 1
📦 Full example manifest (recommended format)
Code
Manifest-Version: 1.0
Fork-Plugin-Class: com.example.myplugin.MyPlugin
Fork-Plugin-Name: MyPlugin
Fork-Plugin-Version: 1.0.0
Fork-Plugin-Author: orangeco
Fork-Plugin-Description: A test plugin for the Fork server.
Fork-Plugin-Depends: CoreLib
Fork-Plugin-SoftDepends: ChatAPI
Fork-Plugin-LoadBefore: WorldTweaks
Fork-Plugin-API-Version: 1
🛠️ How to extend your loader to support these keys
Inside your plugin loader, after reading the manifest:

java
Attributes attrs = jarManifest.getMainAttributes();

String mainClass = attrs.getValue("Fork-Plugin-Class");
String name = attrs.getValue("Fork-Plugin-Name");
String version = attrs.getValue("Fork-Plugin-Version");
String author = attrs.getValue("Fork-Plugin-Author");
String description = attrs.getValue("Fork-Plugin-Description");
String depends = attrs.getValue("Fork-Plugin-Depends");
String softDepends = attrs.getValue("Fork-Plugin-SoftDepends");
String loadBefore = attrs.getValue("Fork-Plugin-LoadBefore");
String apiVersion = attrs.getValue("Fork-Plugin-API-Version");
Then you can store these in a ForkPluginDescription object.
