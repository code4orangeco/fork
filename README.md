🌟 Fork‑Agent Plugin Development Guide A complete guide for creating plugins for the Fork‑Agent modding system.

📦 1. Project Structure Every plugin follows this structure:

Code MyPlugin/ │ ├── src/ │ ├── main/ │ │ ├── java/ │ │ │ └── me/yourname/myplugin/MyPlugin.java │ │ └── resources/ │ │ └── plugin.fork │ └── test/ (optional) │ └── MyPlugin.jar (built output) 🧩 2. Plugin Manifest (plugin.fork) Create:

Code src/main/resources/plugin.fork Paste:

Code name: MyPlugin main: me.yourname.myplugin.MyPlugin version: 1.0.0 author: YourName 🚀 3. Basic Plugin Class Create:

Code src/main/java/me/yourname/myplugin/MyPlugin.java Paste:

java package me.yourname.myplugin;

import me.arjun.forkagent.api.ForkPlugin;

public class MyPlugin implements ForkPlugin {

@Override
public void onLoad() {
    log("Loaded!");
}

@Override
public void onEnable() {
    log("Enabled!");
}

@Override
public void onDisable() {
    log("Disabled!");
}

private void log(String msg) {
    System.out.println("[MyPlugin] " + msg);
}
} 🧵 4. Commands Your plugin system exposes a simple command API:

java @Override public void onEnable() { getCommandRegistry().register("hello", (player, args) -> { player.sendMessage("Hello from MyPlugin!"); }); } Usage in game:

Code /hello 🎧 5. Events Example: Player join event

java @Override public void onEnable() { getEventBus().subscribe(PlayerJoinEvent.class, event -> { event.getPlayer().sendMessage("Welcome to the server!"); }); } Example: Block break event

java getEventBus().subscribe(BlockBreakEvent.class, event -> { log(event.getPlayer().getName() + " broke " + event.getBlock()); }); 🧍 6. Player API Examples:

java Player p = event.getPlayer();

p.sendMessage("Hello!"); p.setHealth(20); p.teleport(100, 64, 100); p.giveItem("minecraft:diamond", 5); ⏱ 7. Scheduler Run a repeating task:

java getScheduler().runRepeating(() -> { log("Tick!"); }, 20); // every 20 ticks Run a delayed task:

java getScheduler().runLater(() -> { log("This ran 5 seconds later!"); }, 100); 📜 8. Custom Logging Your plugin can log with:

java log("Something happened!"); Or advanced logging:

java getLogger().info("Info message"); getLogger().warn("Warning!"); getLogger().error("Error!"); 🧱 9. Custom Items Example: Create a custom sword

java ItemBuilder builder = new ItemBuilder("myplugin:super_sword") .displayName("§bSuper Sword") .damage(10) .unbreakable(true);

getItemRegistry().register(builder.build()); Give it to a player:

java player.giveItem("myplugin:super_sword", 1); 🍳 10. Custom Recipes Example: Craft diamond from dirt:

java Recipe r = new Recipe("myplugin:dirt_to_diamond") .shape("DDD", "DDD", "DDD") .ingredient('D', "minecraft:dirt") .result("minecraft:diamond");

getRecipeRegistry().register(r); 🌐 11. Networking Hooks Send a custom packet:

java getNetwork().send(player, new MyCustomPacket("hello")); Receive packets:

java getNetwork().on(MyCustomPacket.class, (player, packet) -> { log("Received: " + packet.getMessage()); }); 🌍 12. World API Examples:

java World world = getServer().getWorld("world");

world.setBlock(100, 64, 100, "minecraft:diamond_block"); world.spawnEntity("minecraft:zombie", 105, 64, 105); world.setTime(18000); // midnight world.broadcast("Hello world!"); 🛠 13. Building the Plugin Compile:

bash javac -cp ../../fork-agent.jar -d out $(find src/main/java -name "*.java") Package:

bash jar --create --file MyPlugin.jar -C out . -C src/main/resources . Place into:

Code fork-plugins/ 🎉 14. Run the Server bash java -javaagent:fork-agent.jar -jar server.jar You should see:

Code [MyPlugin] Loaded! [MyPlugin] Enabled!
