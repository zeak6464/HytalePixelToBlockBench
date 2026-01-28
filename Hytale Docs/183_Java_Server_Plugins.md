# Java Plugins

Set up a development environment and create **Java plugins** for Hytale. Plugins use the Hytale API to add commands, events, custom logic, and more.

This guide follows the [Hytale Modding](https://hytalemodding.dev) [Setting Up Your Development Environment](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env) approach: JDK 25, Maven, and the official [plugin template](https://github.com/HytaleModding/plugin-template). Hytale Modding lists this under **Server Plugins** and uses the Hytale **Server** dependency; where plugins execute (e.g. server process, local runtime) is defined by the game — see the official docs for details.

---

## Overview

**Asset-based modding** (JSON configs in `Server/`) — items, NPCs, blocks, etc. — is covered in other guides. **Java plugins** are separate: you write Java code, build a JAR, and load it as a plugin. Use them when you need:

> **What about JavaScript?** Hytale does **not** use JavaScript for modding. Plugins are **Java**; content is **JSON**. A **visual scripting** (node-based) system is planned. See [Modding Languages & Scripting](184_Modding_Languages_And_Scripting.md).

- Custom **commands** (beyond [macro commands](167_Macro_Commands.md))
- **Event** listeners (player join, block break, etc.)
- Game **logic** (scheduling, persistence, API calls)
- Integration with **asset mods** (spawn NPCs, give items, etc.)

---

## Prerequisites

- **OS:** Windows 10/11, macOS, or Linux
- **RAM:** 8 GB or more
- **Disk:** ~10 GB free
- **Admin** rights (for installing tools)

---

## 1. Java Development Kit (JDK)

Hytale plugin development uses **Java 25** or later. [OpenJDK](https://adoptium.net/) is recommended.

### Windows

1. Download **OpenJDK 25** from [Adoptium](https://adoptium.net/).
2. Run the installer with default options.
3. Verify:

   ```
   java -version
   ```

### macOS

```bash
brew install openjdk@25
```

If `java -version` fails, add OpenJDK to `PATH`:

```bash
echo 'export PATH="$(brew --prefix)/opt/openjdk@25/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install openjdk-25-jdk
```

---

## 2. IDE

**[IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download/)** is the usual choice for Hytale plugins. Install it and complete the first-run wizard. Other IDEs (Eclipse, VS Code with Java extensions) work too.

---

## 3. Build Tool: Maven

Use **Apache Maven** for dependencies and building. (Gradle is also supported; the [Hytale Modding](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env) site has Gradle examples.)

1. Download Maven: [maven.apache.org/download.cgi](https://maven.apache.org/download.cgi) → e.g. `apache-maven-3.9.12-bin.zip`.
2. Unzip and add the `bin/` folder to your **PATH**.
3. In a **new** terminal:

   ```
   mvn -version
   ```

---

## 4. Clone the Plugin Template

Use the official [Hytale plugin template](https://github.com/HytaleModding/plugin-template):

```bash
git clone https://github.com/HytaleModding/plugin-template.git MyFirstPlugin
cd MyFirstPlugin
```

No Git? Download the [ZIP](https://github.com/HytaleModding/plugin-template/archive/refs/heads/main.zip), extract it, and rename the folder to `MyFirstPlugin`.

---

## 5. Hytale Dependency

Your plugin needs the **Hytale Server** dependency. The template may already reference it. For a **Maven** setup, use the Hytale Maven repos and the `Server` artifact, as in the [official setup guide](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env):

```xml
<project>
    <repositories>
        <repository>
            <id>hytale-release</id>
            <url>https://maven.hytale.com/release</url>
        </repository>
        <repository>
            <id>hytale-pre-release</id>
            <url>https://maven.hytale.com/pre-release</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>com.hypixel.hytale</groupId>
            <artifactId>Server</artifactId>
            <version>2026.01.22-6f8bdbdc4</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
</project>
```

- **`version`:** Use the Hytale build you target. Check [Hytale Modding](https://hytalemodding.dev) or the Hytale Maven repos for current versions.
- **`provided`:** The server supplies the API at runtime; your plugin does not bundle it.

**Gradle** example:

```groovy
repositories {
    mavenCentral()
    maven { name = "hytale-release"; url = uri("https://maven.hytale.com/release") }
    maven { name = "hytale-pre-release"; url = uri("https://maven.hytale.com/pre-release") }
}

dependencies {
    compileOnly("com.hypixel.hytale:Server:2026.01.22-6f8bdbdc4")
}
```

---

## 6. Plugin Manifest

The template includes `src/main/resources/manifest.json`. This tells the Hytale server how to load your plugin:

```json
{
  "Group": "dev.hytalemodding",
  "Name": "ExamplePlugin",
  "Version": "1.0.0",
  "Description": "Description of your plugin",
  "Authors": [
    {
      "Name": "Your Name",
      "Email": "your.email@example.com",
      "Url": "https://your-website.com"
    }
  ],
  "Website": "https://your-plugin-website.com",
  "ServerVersion": "*",
  "Dependencies": {},
  "OptionalDependencies": {},
  "DisabledByDefault": false,
  "IncludesAssetPack": false,
  "Main": "dev.hytalemodding.ExamplePlugin"
}
```

| Field | Purpose |
|-------|---------|
| **Group** | Plugin namespace (e.g. your domain). |
| **Name** | Plugin display name. |
| **Version** | Semantic version. |
| **Main** | Fully qualified class name of your **main plugin class** (implements the plugin interface). |
| **ServerVersion** | `*` = any server version, or a specific version constraint. |
| **Dependencies** | Required plugins (by Group:Name). |
| **IncludesAssetPack** | `true` if the plugin JAR contains asset packs. |

Edit **Group**, **Name**, **Description**, **Authors**, **Website**, and **Main** to match your plugin. **Main** must point to your main class.

---

## 7. Open in IntelliJ

1. **File → Open** → select your `MyFirstPlugin` folder.
2. IntelliJ detects the Maven project and imports it.
3. Wait for indexing and dependency resolution.

If the Hytale **Server** dependency is missing, add it to `pom.xml` (or `build.gradle`) as in **§5** and refresh Maven/Gradle.

---

## 8. Project Layout

Typical layout:

```
MyFirstPlugin/
├── pom.xml                 # Maven config, Hytale dependency
├── src/
│   └── main/
│       ├── java/
│       │   └── dev/hytalemodding/
│       │       └── ExamplePlugin.java   # Main plugin class
│       └── resources/
│           └── manifest.json
└── target/                 # Build output (after mvn package)
    └── MyFirstPlugin-1.0-SNAPSHOT.jar
```

Your **main class** (e.g. `ExamplePlugin`) usually:

- Implements the plugin lifecycle interface (e.g. `onEnable` / `onDisable`).
- Registers **commands**, **events**, or other handlers.

Exact interfaces and names depend on the current Hytale API; see [Hytale Modding](https://hytalemodding.dev) and the template.

---

## 9. Build and Run

**Build the JAR:**

```bash
mvn clean package
```

The JAR is in `target/`. Copy it into your Hytale **plugins** folder (or wherever your setup expects plugin JARs). Restart or reload plugins as required, then check logs to confirm your plugin loads.

**Testing:**

- Use a **local Hytale** instance and enable your plugin.
- See [Build and Test Your Mod](https://hytalemodding.dev/en/docs/guides/plugin/build-and-test) on Hytale Modding for details.

---

## 10. Next Steps

Once the template runs:

1. **Rename** the plugin in `pom.xml` and `manifest.json`.
2. **Implement** your main class and hook into the Hytale API (commands, events, etc.).
3. **Use official docs** for the current API:
   - [Setting Up Your Development Environment](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env)
   - [Creating Commands](https://hytalemodding.dev/en/docs/guides/plugin/creating-commands)
   - [Creating Events](https://hytalemodding.dev/en/docs/guides/plugin/creating-events)
   - [Build and Test Your Mod](https://hytalemodding.dev/en/docs/guides/plugin/build-and-test)

**Asset modding** (items, NPCs, blocks, etc.) stays in `Server/` (JSON configs). Java plugins can **use** those assets (spawn NPCs, give items, etc.) via the server API. See [Server Modding](12_Server_Modding.md), [Creating Commands](41_Commands.md) (macro commands), and the rest of the docs for asset-based content.

---

## Quick Reference

| Resource | URL |
|----------|-----|
| **Hytale Modding** | [hytalemodding.dev](https://hytalemodding.dev) |
| **Plugin setup** | [Setting Up Your Development Environment](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env) |
| **Plugin template** | [github.com/HytaleModding/plugin-template](https://github.com/HytaleModding/plugin-template) |
| **OpenJDK** | [adoptium.net](https://adoptium.net/) |
| **Maven** | [maven.apache.org](https://maven.apache.org/) |
| **IntelliJ IDEA** | [jetbrains.com/idea](https://www.jetbrains.com/idea/download/) |

---

## Troubleshooting

- **`java -version` not found** — Add the JDK `bin/` directory to your **PATH**.
- **`mvn` not found** — Add Maven’s `bin/` to **PATH** and use a new terminal.
- **Hytale API missing / compile errors** — Ensure the **Server** dependency is in `pom.xml` (or Gradle), with correct `version` and `repositories`. Refresh Maven/Gradle.
- **Plugin not loading** — Check **Main** in `manifest.json` matches your main class, and the JAR is in the plugins folder. Check logs for errors.

---

**Previous:** [Advanced Combat Mechanics](182_Advanced_Combat_Mechanics.md) | **Next:** [Modding Languages & Scripting](184_Modding_Languages_And_Scripting.md)

**See also:** [Server Modding](12_Server_Modding.md) · [Creating Commands](41_Commands.md) · [Macro Commands](167_Macro_Commands.md)
