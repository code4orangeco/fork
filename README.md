# fork
fork is a server software that takes the mojang jar and edits it
you run the server with java -javaagent:<your server name wihtout the lessthan and greaterthan signs> -jar server.jar
to run simeple plugins like package myplugin;

import com.example.forkagent.ForkPlugin;
import com.example.forkagent.events.PlayerJoinEvent;
import com.example.forkagent.events.ForkListener;

public class MyTestPlugin extends ForkPlugin implements ForkListener {

    @Override
    public void onEnable() {
        System.out.println("[MyTestPlugin] Enabled!");
        getEventBus().registerListener(this);
    }

    @Override
    public void onTick() {
        // Runs every server tick
    }

    @ForkListener.EventHandler
    public void onJoin(PlayerJoinEvent event) {
        System.out.println("Player joined: " + event.getPlayer());
    }
}


use this file structure 
server/
  server.jar
  fork-agent-1.1.0.jar
  fork-plugins/
      MyPlugin.jar
