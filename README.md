# Download RDCMan (Remote Desktop Connection Manager)

Updated: August 28, 2025       
**[Download Remote Desktop Connection Manager]()**

## Introduction

**Remote Desktop Connection Manager (RDCMan)** is built to simplify the handling of multiple remote desktop sessions, making it a crucial tool for managing server environments, automated kiosk systems, and data centers where consistent access to individual machines is necessary.

Servers are organized into labeled groups, which allow users to connect or disconnect all servers within a group at once. A thumbnail display provides real-time previews of currently active sessions. Logon credentials can be inherited from parent groups or managed centrally, easing the complexity of password administration—updates only need to be applied once. RDCMan securely handles stored credentials using encryption (via either the local user’s credentials through `CryptProtectData` or an X509 certificate).

Users running operating systems older than Windows 7 or Vista must install version 6 of the Terminal Services Client, which is available from the Microsoft Download Center: XP; Win2003.

**Upgrade note:** RDG files created with this version will not work with older RDCMan versions. When legacy files are opened and then saved, a backup copy with the `.old` extension is automatically created.



## The Display

The RDCMan interface consists of a menu bar, a hierarchical server group tree, a splitter, and the client display area.

### The Menu

Several top-level menus are available in RDCMan:

* **File** – for loading, saving, and closing RDCMan group files
* **Edit** – enables adding, deleting, and modifying server or group settings
* **Session** – provides options to connect, disconnect, or log off from sessions
* **View** – manages the visibility of the server tree, virtual groups, and adjusts the client display area
* **Remote Desktops** – offers a hierarchical view of groups and servers, especially handy when the Server Tree is hidden
* **Tools** – used to configure application-wide settings
* **Help** – displays information about RDCMan (you’ve likely already located this section)

### The Tree

Common actions like adding, removing, and configuring servers or groups are performed by right-clicking on a node in the tree. Items can also be reordered by dragging and dropping.

**Keyboard shortcuts include:**

* **Enter**: Connect to the selected server
* **Shift+Enter**: Connect using the "Connect As" feature
* **Delete**: Remove the selected server or group
* **Shift+Delete**: Remove the selected item without confirmation
* **Alt+Enter**: Open the properties window for the selected server or group

To position the server tree on either side of the window, use the **\[View\.Server tree location]** menu.

Server tree visibility can be toggled between docked, auto-hide, or hidden entirely using the **\[View\.Server tree visibility]** option. If hidden, servers are still accessible via the Remote Desktops menu. In auto-hide mode, a splitter remains visible on the left side—hovering over it reveals the tree again.

### The Client Area

What appears in the client area depends on the selected tree node. Selecting a server displays that server’s remote desktop session. Selecting a group displays thumbnail views of its servers. The size of this area is adjustable via the View menu or by resizing the RDCMan window itself. To disable resizing from the window edges, use **\[View\.Lock window size]**.

**Note:** When navigating with the keyboard inside the thumbnail view, active focus may shift to a connected session. Be mindful, as it may not always be obvious which server is in focus. This can be controlled with the setting: **\[Display Settings.Allow thumbnail session interaction]**.

### Full Screen Mode

To enter full screen with a server, select it and press **Ctrl+Alt+Break** (this shortcut can be changed in the Shortcut Keys settings). To exit full screen mode, press the same keys again or use the minimize/restore controls in the connection's title bar. Multi-monitor support is available if monitor spanning is enabled.

### Shortcut Keys

A complete list of Terminal Services shortcut keys is accessible through the interface. Many can be customized under the **Hot Keys** tab.



## Files

The foundational structure in RDCMan is the Remote Desktop file group. A file group consists of servers and/or groups contained within one physical file. Servers cannot exist outside of a group, and groups must be part of a file.

A file behaves like a server group but cannot change its parent.



## Groups

Groups include collections of servers and configuration options like logon details. These settings can inherit values from other groups or from application defaults. Groups can be nested but are uniform: they contain only servers or only other groups, not both at the same level. All servers in a group can be connected or disconnected in bulk.

When a group is selected in the tree, its servers are shown as thumbnails. Thumbnails can show the actual remote windows or just status indicators. Global thumbnail settings are managed in **\[Tools.Options.Client Area]**, while display options for specific servers or groups reside in the Display Settings panel.

### Smart Groups

Smart groups populate automatically according to predefined criteria. Any sibling group’s ancestor qualifies for inclusion.

### The Reconnect Virtual Group

At times, a server may become disconnected and remain offline for an extended period—such as during a system reboot following an OS update. In these scenarios, you can move the affected server to the Reconnect group. RDCMan will continuously attempt to re-establish the connection until it's successful.

The Reconnect group can be shown or hidden via the **View** menu.

### The Favorites Virtual Group

The Favorites virtual group presents a flat list of servers you've marked as favorites. You can include any server from the tree view. This is especially helpful if you manage numerous servers but frequently interact with only a select few across different groups.

You can toggle the visibility of the Favorites group using the **View** menu.

### The Connect To Virtual Group

This virtual group contains servers that aren't assigned to any manually created groups. For more details, refer to the **Ad Hoc Connections** section.

The Connect To group becomes visible when ad hoc connections exist and disappears when there are none.

### The Recent Virtual Group

The Recent virtual group holds servers that were accessed most recently.

Use the **View** menu to toggle the visibility of the Recent group.


### Adding Servers Manually

When server names follow a structured pattern, you can add multiple entries at once. RDCMan supports two types of patterns:

* **Iteration** – Use `{a,b,c}` to iterate through a comma-separated list
* **Range** – Use `[1-5]` to iterate across a numeric range; include leading zeros to set the minimum digit width

**Examples:**

* `server1{a,b,c}`: Expands to `server1a`, `server1b`, `server1c`
* `server[001-15]`: Expands to `server001`, `server002`, ..., `server015`
* `{dca,dcb}rack[1-5]sql[1-2]`: Generates `dcarack1sql1`, `dcarack1sql2`, ..., `dcarack5sql2`, `dcbrack1sql1`, ..., `dcbrack5sql2`

### Importing Servers from a Text File

Servers can also be brought into a group using a text file. The format is straightforward—each line contains a single server name:

```txt
Server1
SecondServer
YANS
```

Alternatively, server names can be entered manually into the dialog box.

All imported servers are added to the same group using a shared set of preferences. If a server being imported already exists by name, its settings will be updated with the new values.

### Ad Hoc Connections

You can initiate ad hoc server connections through the **\[Session.Connect to]** option. These connections will appear in the Connect To virtual group. You can convert these temporary entries into permanent ones by moving them to a user-defined group. If left in the Connect To group, they will not be saved when RDCMan is closed.

### Windows Azure

Within the **\[Connection Settings]** tab, input the role name and instance name into the *Load balance config* field as described [here](/en-us/azure/cloud-services/cloud-services-role-enable-remote-desktop-powershell). For example:

```
Cookie: mstshash=MyServiceWebRole#MyServiceWebRole_IN_0#Microsoft.WindowsAzure.Plugins.RemoteAccess.Rdp
```

### Session Actions

While connected to a session, you can shift focus to another session or return to the server tree.

* **Focus release left** (default: **Ctrl+Alt+Left**) – Switches to the previously selected session
* **Focus release right** (default: **Ctrl+Alt+Right**) – Opens a dialog with options to focus on one of the most recently used sessions, the server tree, or to minimize RDCMan

Some keyboard shortcuts and system-level commands can be challenging to send to a remote session—especially if RDCMan itself is running inside another remote session (e.g., **Ctrl+Alt+Del**). These actions are available through the **\[Session.Send keys]** and **\[Session.Remote actions]** menus.

## Global Options

The **\[Tools.Options]** menu item opens the Options Dialog, where global configuration settings can be adjusted. General settings such as client area dimensions are modifiable here. Note that most server-specific configurations—like hot keys or settings on the experience tab—only take effect the next time the server is reconnected.

### General

**Hide main menu until ALT is pressed**
This setting hides the main menu bar until either the ALT key is pressed or the window title bar is clicked.

**Auto save interval**
RDCMan offers automatic file saving at defined time intervals. To enable this, turn on auto-save and set the desired frequency (in minutes). Setting the interval to 0 disables periodic saving but also suppresses the prompt to save on exit.

**Prompt to reconnect previously connected servers at launch**
RDCMan keeps track of servers that were connected when the application was last closed. Upon restarting, it prompts you to choose which of those servers to reconnect. Disabling this setting causes all previously connected servers to reconnect automatically. See the **Command Line** section for switches that can override this behavior.

**Default group settings**
Clicking this button opens a dialog that allows you to define default values at the root of the inheritance chain. These base settings apply wherever a File group inherits from its parent container.

### Tree

**Click to select also sets focus to remote client**
Normally, clicking on a server node in the tree keeps keyboard focus within the tree. Enabling this option causes focus to switch to the corresponding remote session.

**Dim tree nodes when tree control is inactive**
When enabled, RDCMan dims the server tree while it's not in focus, helping to visually distinguish where keyboard input is currently directed.

### Client Area

**Client Area Size**
This controls the overall size of RDCMan’s client display area. These settings are also available via the **\[View\.Client size]** menu.

**Thumbnail Unit Size**
The size of each thumbnail can be set using either a fixed pixel value or a relative percentage of the total width of the client area panel.

### Hot Keys

Many hot key combinations used in remote desktop sessions can be reconfigured, though certain constraints apply. For instance, if a default hot key includes ALT, the custom one must also contain ALT. To assign a new hot key, place the cursor in the relevant box and press the desired key combo.

### Experience

Depending on your network bandwidth, you may want to disable certain Windows visual effects to improve performance. You can either select a preset connection speed, which adjusts all settings at once, or customize them individually. Options include disabling desktop backgrounds, menu and window animations, full content drag, and Windows visual themes.

### Full Screen

**Show full screen connection bar**
**Auto-hide connection bar**
In full-screen mode, the remote desktop ActiveX component shows a connection bar at the top of the screen. This bar can be turned on or off. If enabled, it can either stay visible or auto-hide when not in use.

**Full screen window always stays on top**
This option ensures that the full-screen session window remains in the foreground above all other windows.

**Enable multi-monitor support when needed**
By default, a full-screen session is limited to the monitor it launches on. You can activate support for spanning multiple monitors via these settings. If the remote desktop resolution exceeds that of the current monitor, the session will expand across adjacent screens. Note: spanning only works across rectangular regions—when monitors have varying vertical resolutions, the session adjusts to the shortest height. Also, the maximum resolution for the remote desktop control is limited to 4096x2048.



## Local Options

Groups and individual servers feature a series of tabbed property pages that provide various configuration choices. Most of these tabs are common between groups and servers. If the **"Inherit from parent"** checkbox is selected, the subsequent settings are inherited from the parent container. Many server-side changes, such as desktop resolution, will only take effect after reconnecting to the server.

### File Settings

This tab is available only when editing the properties of a file group. It allows setting the file's group name, displays the full (read-only) file path, and includes a comments section for user notes.

### Group Settings

This tab appears only when viewing or editing the properties of a group. It includes fields for setting the group’s name, selecting its parent (for nesting purposes), and entering an optional comment.

### Server Settings

This page is exclusive to the properties of individual servers. It allows you to configure the server’s name, optional display name, parent grouping, and a comment field. If you're working with SCVMM virtual machines, you can connect via RDP to the host using the VM console connect feature. To retrieve the VM ID, run the following PowerShell command:

```powershell
get-vm | ft ElementName,Name,Id
```

This will return the identifier needed to establish the connection.

### Logon Credentials

The **Logon Credentials** tab manages login-related settings for remote sessions. Here you can specify the user name, password, and domain. It’s also possible to enter the domain and user in a combined format like `domain\user`.

If the "domain" isn’t a Windows domain (such as in workgroup environments), you may use `\[server]` or `\[display]` as placeholders. These are dynamically replaced with the server name or display name, respectively, during the login process. This is especially useful when managing machines where administrator access is required.

The logon information set here acts as the default for future connections. To override these settings on a one-time basis, use the **Connect As** option from the session menu.

### Gateway Settings

The **Gateway Settings** page configures access through a Terminal Services Gateway (TS Gateway) server. You can define the gateway’s address, choose an authentication method, and enable or disable local address bypass.

For users on Windows Vista SP1, Windows Server 2008 (Longhorn Server), or later, additional features are available:

* Manually specify credentials for the gateway server
* Choose to share gateway credentials with the target remote server

### Connection Settings

This tab provides controls over how the remote session is established and what actions occur after logging in.

You can choose whether to connect to the console session and specify the RDP port for the connection.

It’s also possible to configure RDCMan to run a specific program immediately upon login by entering the program’s path and (optionally) a working directory. Keep in mind, however, this applies only when connecting to the console session for the first time. Reconnecting or initiating non-console sessions will not trigger the program launch. This behavior reflects how Terminal Services handles such scenarios in practice.

### Remote Desktop Settings

The **Remote Desktop Settings** page allows you to define the resolution of the remote session’s desktop. This sets the logical size of the desktop environment, which may differ from the client’s visible area.

For instance, if you configure the remote desktop to 1280×1024 and the RDCMan client window is 1024×768, scroll bars will appear to navigate the full desktop. If your client view is larger (e.g., 1600×1200), the entire desktop will fit, surrounded by unused space.

Selecting **"Same as client area"** makes the remote desktop match the RDCMan client area (excluding the server tree). Choosing **"Full screen"** resizes it to fit the entire screen where the session is displayed.

Note: The remote desktop size is determined when the session starts. Modifying the setting while connected won’t resize the session.

The maximum supported desktop size depends on the Remote Desktop ActiveX control version in use. Version 5 (pre-Windows Vista) supports up to 1600×1200. Version 6 (Vista and later) extends support up to 4096×2048. These limits are enforced during the actual connection rather than at the configuration stage, which helps maintain compatibility with RDCMan files shared across multiple systems.

### Local Resources

Various resources from the remote server can be redirected back to the client machine. For example, audio playback can be configured to occur on the local system, remain on the remote server, or be turned off entirely. Key combinations involving the Windows key (e.g., **Alt+Tab**) can be applied exclusively to the client, strictly to the remote session, or dynamically—targeting the remote session in full screen mode and the client when in windowed mode.

Additionally, client-side devices such as drives, ports, printers, smart cards, and the clipboard can be shared automatically with the remote machine during a session.

### Security Settings

This section allows you to define whether the remote computer must be authenticated before a connection is established.

### Display Settings

This page provides customization options for thumbnail previews of remote sessions.

The main control here is **thumbnail scale**, which determines how much space is allocated to each server’s thumbnail. The default scale is 1, but this can be increased for servers that require greater visibility—for instance, using a scale of 3 or 5 enhances the usability of those sessions within the thumbnail panel, while still showing other sessions alongside. Note: this setting applies only to individual servers.

Groups offer three additional display settings:

* **Preview session in thumbnail** – Enables live, continuously updated previews of server activity.
* **Allow thumbnail session interaction** – If previews are enabled, this allows direct interaction within the thumbnail window.
* **Show disconnected thumbnails** – Controls whether disconnected servers are still shown in the thumbnail panel.

### Encryption Settings

RDCMan can encrypt saved passwords using either the local user account (via `CryptProtectData`) or an X509 certificate. The **Encryption Settings** tab is accessible within both the Default Group Settings and File Settings dialogs.

Any personal certificates in the current user's store that include a private key are valid for use. To generate one, you can run the following PowerShell command:

```powershell
New-SelfSignedCertificate -KeySpec KeyExchange -KeyExportPolicy Exportable -HashAlgorithm SHA1 -KeyLength 2048 -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=MyRDCManCert"
```

This creates a certificate named "`MyRDCManCert`" in the Personal Certificate Store of the current user. If you plan to use this certificate on another system, ensure it is exported along with the private key.

### Profile Management

This tab enables you to create, modify, or delete credential profiles used for authentication during remote sessions.



## List Remote Sessions

RDCMan includes basic functionality for listing remote sessions that were not launched through it. This is accessed using the **\[Session.List Sessions]** menu item.

Important: The user account running RDCMan must have **Query Information** privileges on the target remote server to retrieve session details. Additionally, the server must be directly reachable—connections routed through a gateway server are not supported. In order to use **Disconnect** or **Logoff**, those corresponding permissions must also be granted. For further details, refer to MSDN’s documentation on Remote Desktop permissions.



## Command Line

By default, RDCMan automatically reopens any files that were open when the application was last closed. You can change this behavior by providing specific files as command-line arguments. The following additional command-line switches are also supported:

* `/reset` – Resets stored application preferences such as window position and size.
* `/noopen` – Launches RDCMan without reloading previously opened files; starts with a blank environment.
* `/c server1[,server2...]` – Connects directly to the specified servers.
* `/reconnect` – Automatically reconnects to all servers that were connected at shutdown, without prompting.
* `/noconnect` – Skips the prompt to reconnect servers from the last session.

## Find Servers

RDCMan provides a dialog for locating servers, accessible via **Ctrl+F** or the **Edit > Find (servers)** menu command. All servers that match a specified regular expression pattern are listed in the dialog. You can interact with these servers through the context menu. The match is evaluated against the server's full name, which includes its group path (`group\server`).



## Credential Profiles

Credential profiles allow logon information to be stored either globally or within an individual RDCMan file. This enables consistent reuse of credentials across unrelated groups that don’t share a common parent in the tree. A common use case is to centralize credentials for both servers and gateways, so that when a password changes, only one update is required.

Credential profiles also make it easier to share RDG files among multiple users. Since RDCMan encrypts credentials in a user-specific way, embedding passwords directly in shared files isn't practical. Instead, users can create a credential profile—such as "Me"—in their global credential store.

You can update an existing credential profile in two ways:

1. Open the credentials dialog, enter the same name and domain, and save it back to the same store (global or file). You’ll be prompted to update the existing profile.
2. Use the **Profile Management** tab within the group properties for either the global or file credential store.

Passwords in file-scoped profiles are encrypted according to the file’s **Encryption Settings**. Global profiles, on the other hand, use the settings specified in the **Default Group Settings**.



## Policies

RDCMan can apply policy restrictions defined in the Windows Registry at the following key:
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\RDCMan`

* **DisableLogOff** – Add this `DWORD` entry and set it to any non-zero value to disable the log-off command throughout RDCMan.

## FAQ

* **How can I log in using smartcard credentials?**
  In the **Local Resources** tab, enable the option **Redirect smart cards**.

* **I’m getting an error when connecting through a gateway (e.g., Error 50331656). What causes this?**
  Make sure the gateway is specified using its fully qualified domain name.

* **How can I enable automatic logon?**
  You’ll need to modify Group Policy settings. Open the **Group Policy MMC Snap-in** and go to:
  `Local Computer Policy > Computer Configuration > Administrative Templates > Windows Components > Terminal Services > Encryption and Security`
  Then double-click **Always prompt client for password upon connection** and set it to **Disabled**.

* **Is it possible to resize the remote desktop session while connected?**
  No, resizing the desktop requires disconnecting and then reconnecting. Use the **Reconnect** feature to do this seamlessly. In **Display Settings**, servers can be configured to auto-reconnect using the updated resolution, whether docked or undocked.
