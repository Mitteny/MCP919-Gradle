# What is this?

This is a ready-to-run vanilla Minecraft 1.8.9 client. It uses Gradle to manage dependencies and build. It is completely safe and vanilla, I didn't modify any of the code, you can add your own modifications and build your own client. If you want to decompile Minecraft 1.8.9 and set up it with Gradle like me, go to **How to create this myself** section.  



# How to run

Copy the repository first.

<br>

In order to run Minecraft, You should add a run configurationin Intellij IDEA. Choose application and type in the main class `Start`, then you add a VM Option (alt+v):  `-Djava.library.path=..\natives` and set working directory to `run`, you might also need to specify the classpath to the main one. 

<br>

After all these, you should get a run configuration looks like this. <img src="https://i.imgur.com/vUHwDpm.png"/>

And you can finally run the client.

# How to export?

Just use gradle to build this project and you'll find the jar file under `build\libs`. Keep in mind that the jar file you produce isn't obfuscated.

# How to create this myself?

1. Download mcp918.zip from [MCP Official Website](http://www.modcoderpack.com).

2. You extract it into any folder (we call this folder `mcp` in this passage) and change versions in `mcp\conf\version.cfg` to 1.8.9. 

   <br>

   You then download `mcp_stable_nodoc-22-1.8.9.zip` and `mcp-1.8.9-srg.zip` from [MCPMappingsArchive](https://github.com/ModCoderPack/MCPMappingsArchive) (located under *mcp_stable_nodoc/22-1.8.9/mcp_stable_nodoc-22-1.8.9.zip* and *mcp/1.8.9*) and extract them all into `mcp\conf`, overwriting all the original files. 

3. You go back to `mcp` and run decompile.bat (make sure you've ran Minecraft at least once so that you have all the assets file in `%APPDATA%\.minecraft`).

4. Then, there should be an error when MCP tries to recompile Minecraft. Don't worry, you can fix it by changing codes in  `mcp\src\minecraft\net\minecraft\world\gen\structure\StructureMineshaftPieces.java` from

   ```java
   for (lvt_5_1_ = 0; k < this.boundingBox.getZSize(); k = k + 4)
   ```

   to 

   ```java
   for (int k = 0; k < this.boundingBox.getZSize(); k = k + 4)
   ```

   Then, you've successfully fixed the error!

5. Next, you create a new Gradle project in Intellij IDEA and there should be a `src\main\java` directory in your `project` root directory, copy all the decompiled and deobfuscated Minecraft code (`mcp\src\minecraft`) into this directory.

6. Minecraft still can't run without its libraries and assets, so we're going to add them to our Gradle project.

   <br>

   First, we create a `run` folder, a `natives` folder, a `libs` folder under `project` root directory. 

   <br>

   Copy all the .jar files from `mcp\jars\libraries` to `project\libs` and add these lines to your Gradle `build.gradle`:

   ```groovy
   dependencies {
       implementation fileTree('libs')
   }
   ```

   After that, you copy all the natives file from `mcp\jars\versions\1.8.8\1.8.8-natives` to `project\natives`. 

   <br>

   Next, we locate `jars\versions\1.8.9\1.8.9.jar` and open it as a zip file, copy `assets`, `log4j2.xml` and `pack.png` to `project\src\main\resources`. Then, we go back to `mcp\jars` and copy the `assets` folder to `mcp\run`.

7. Lastly, if you followed all these steps, you should have set up a runnable vanilla minecraft 1.8.9 client with Gradle. You just simply follow the guide from **How to run** and you'll be able to run this client.
