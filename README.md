# fork
fork is a server software that takes the mojang jar and edits it
you run the server with java -javaagent:<your server name wihtout the lessthan and greaterthan signs> -jar server.jar

how to make a fork plugin:
because you built the entire Fork plugin architecture yourself. A Fork plugin is just:

A JAR file

With a MANIFEST.MF containing Fork metadata

And a main class that implements your ForkPlugin interface

Loaded by the Fork Agent at runtime

Receiving onEnable(), onTick(), and events from your event bus

Below is a complete, structured guide with copy‑and‑paste code, folder layout, and build commands.

🧱 1. Folder structure for a Fork plugin
Code
my-fork-plugin/
 ├─ src/
 │   └─ main/
 │       └─ java/
 │           └─ com/example/myplugin/
 │               └─ Main.java
 └─ pom.xml
🧩 2. The ForkPlugin interface (from your agent)
Your plugin must implement:

java
package com.forkserver.plugin;

public interface ForkPlugin {
    void onEnable();
    void onTick();
    void setDescription(ForkPluginDescription description);
    ForkPluginDescription getDescription();
}
🧠 3. Create your plugin main class
src/main/java/com/example/myplugin/Main.java

java
package com.example.myplugin;

import com.forkserver.plugin.ForkPlugin;
import com.forkserver.plugin.ForkPluginDescription;

public class Main implements ForkPlugin {

    private ForkPluginDescription description;

    @Override
    public void onEnable() {
        System.out.println("[MyPlugin] Enabled! Version: " + description.getVersion());
    }

    @Override
    public void onTick() {
        // Runs every server tick
        // System.out.println("[MyPlugin] Tick!");
    }

    @Override
    public void setDescription(ForkPluginDescription description) {
        this.description = description;
    }

    @Override
    public ForkPluginDescription getDescription() {
        return description;
    }
}
📦 4. Add MANIFEST.MF entries (Fork plugin metadata)
Your Fork Agent reads plugin metadata from the JAR manifest.

Add this to your Maven pom.xml:

xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.0</version>
            <configuration>
                <archive>
                    <manifestEntries>
                        <Fork-Plugin-Class>com.example.myplugin.Main</Fork-Plugin-Class>
                        <Fork-Plugin-Name>MyPlugin</Fork-Plugin-Name>
                        <Fork-Plugin-Version>1.0.0</Fork-Plugin-Version>
                        <Fork-Plugin-Author>orangeco</Fork-Plugin-Author>
                        <Fork-Plugin-Description>A test plugin for Fork.</Fork-Plugin-Description>
                    </manifestEntries>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
These keys are required by your loader.

🛠 5. Build the plugin
From inside your plugin folder:

bash
mvn clean package
Your plugin JAR will appear in:

Code
target/my-fork-plugin-1.0.0.jar
📁 6. Install the plugin into Fork
Copy the JAR into your Fork plugin folder:

bash
cp target/my-fork-plugin-1.0.0.jar ~/fork-agent/fork-plugins/
🚀 7. Run the server with the Fork Agent
From your server directory:

bash
java -javaagent:fork-agent/target/fork-agent-1.1.0.jar -jar server.jar
You should see:

Code
[MyPlugin] Enabled! Version: 1.0.0
🎉 8. Optional: Listen to events
Example: PlayerJoinEvent

java
ForkPluginManager.getInstance()
    .getEventBus()
    .register(PlayerJoinEvent.class, event -> {
        System.out.println("[MyPlugin] Player joined: " + event.getPlayerName());
    });
Put this inside onEnable().









use this file structure 
server/
  server.jar
  fork-agent-1.1.0.jar
  fork-plugins/
      MyPlugin.jar (name can be anything)

      for the fork software i do not test that often so tell me if it does not work
in discord by pinging me @dev_debiancoder
