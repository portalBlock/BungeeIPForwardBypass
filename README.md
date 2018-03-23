# About
This plugin/patch combination allows you to spoof Bungeecord IP forwarding data to login to improperly configured servers.
The vanilla Minecraft protocol sends the hostname (ex: play.example.com) players connect with to the server as part of the login
process. Bungeecord utilizes this information to direct your players to different servers (creative.example.com, etc.) but also
uses this to forward player data to Spigot servers.
## How Does it Work?
In addition to the hostname, Bungeecord appends the IP address and UUID of players. This data is then injected into the Player objects
to create seamless compatibility with Bukkit plugins. When a plugin calls Player#getAddress(), the 'spoofed' address provided by Bungee
is returned. Spigot patches add a way to get the actual socket address using Player#getRawAddress() and PlayerLoginEvent#getRealAddress().
This is important to note, as plugins that do not call these functions will always return the address provided by Bungeecord.


The data can be spoofed using a Bungeecord patch to alter the information that is sent. The patch supplied in this repo enables
modification of the hostname, hostname port, and IP address. The accompanying plugin wraps the new features with easy to use commands.
Since Spigot has no native way of knowing if the information is coming from a trusted Bungeecord instance, it blindly accepts the data.
## How can I Prevent This?
If you are running a Spigot server in Bungeecord mode (bungeecord: true in spigot.yml), you are potentially affected so continue reading.
At this point you must ensure your a protected by either


* A) Using a firewall (such as IPTables) to block incoming connections to the
Spigot server's port from untrusted connections. Please see Spigot's firewall guide [here](https://www.spigotmc.org/wiki/firewall-guide/).
* B) Another option is to use a Spigot plugin to whitelist IP addresses. You will only want to whitelist your proxy addresses. You must
be careful with the plugins you choose here. Plugins designed to provide an IP whitelisting system similar to player whitelisting may be
bypassed if they call Player#getAddress(). You need to ensure the plugin you pick uses the Spigot patches to retrieve the player's IP.


You may use this software to test IP whitelisting plugins, or your firewall configuration.
## Publishing this is stupid! People will use it maliciously!!
As long as air exists, people with bad intentions will exist. I chose to publish this because I thought it was interesting and to draw
attention to it so server owners would secure their servers. I've talked with an administrator from Spigot and Bungeecord and they are
aware of this 'issue' and feel that properly configured servers is enough mitigation. I completely agree with this.


I will be clear though, I do not condone the use of this plugin for anything more than testing the security of your own server, or
a server for which you have been authorized to test. Any malicious use of this code is strictly prohibited.
## How do I use this?
You'll need to be comfortable compiling Java applications using Maven. Start by cloning this repository, as well as Bungeecord's.
Copy the patch file from the root of this repository to the root of this repo. Apply the patch using 'git apply <patch name>'.
You can now compile Bungeecord using this instructions and copy the final Bungeecord.jar to a folder named 'libs' in the root of this
repo. Compile the plugin using 'mvn package'. You can then install the plugin in Bungeecord as you normally would, and proceed to run
Bungee. Note: You will need a Spigot server running so Bungee has somewhere to send you when you connect. No plugins are required on
that server.


The commands are /spoof and /connect. /connect will work for completely unprotected servers. /spoof will work to get around plugins
that filter based on 'hostname[:port]' or Player#getAddress() (Not using Spigot patches).
### These Instructions Confuse Me!!!
This isn't designed for the average person, so I didn't go into detail. If you can't figure out what you need from this document and
the source code, you probably shouldn't be using this.