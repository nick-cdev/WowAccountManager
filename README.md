# Wow Account Manager
This is a multi-tier C# Windows Forms desktop client and .NET 8 Minimal API cloud platform engineered to automate, monitor, and synchronize multiple World of Warcraft (WoW) account processes concurrently.It automates the login process, maintains active sessions via anti-AFK mechanics, and provides a multi-threaded key-spamming utility.  

The system leverages a decoupled, asynchronous client-server pipeline to stream real-time desktop worker telemetry directly into a hosted Supabase PostgreSQL instance, providing seamless fleet management through an auto-refreshing, web monitoring dashboard

<p align="center">
  <img src="./assets/img2.gif" alt="App Demo">
</p>

## 🚀 Key Features
## 🌐 Secure Cloud Sync & Telemetry Engine

* **Automated Cloud Synchronization**: Exports and matches local account structures from the XML engine directly into the cloud backend during client startup initialization.
* **Real-Time Telemetry Streaming**: Background worker loops fire zero-latency status updates to a remote logger without interrupting desktop thread execution.
* **Parent-Child Entity Relational Tracking**: Logs discrete automation events (e.g., bot logins, anti-AFK procedure activation) directly under a strict foreign key mapping linked to a specific character.
* **Live "Time-Ago" Web Monitoring Dashboard**: Displays active character profiles inside a dark-mode web console featuring live-updating relative timestamp indicators.
* **Self-Pruning Maintenance Routine**: Automates backend database hygiene by wiping history logs older than 7 days every time a new client connection initializes.

## 🔒 Enterprise-Grade Security Architecture

* **Centralized Endpoint Filter Grouping**: Implements a dedicated API-wide route routing system that intercepts traffic and applies uniform validation rules across restricted paths.
* **Decoupled Secret Configuration Management**: Safely isolates sensitive database credentials and validation tokens inside ignored `appsettings.json` and `App.config` files to eliminate source control credential leaks.
* **Enforced TLS 1.2/1.3 Handshake Layer**: Upgrades legacy HTTP client network protocols to match modern cryptographic traffic transit constraints over the public web.

## ⚙️ How It Works

* **Automated Account Management**: Allows users to select an account from a stored list for instant login or seamless switching between active game windows.  
![App Demo](./assets/Animation4.gif)
*Note: Visual demonstrations were captured in a controlled environment for the purpose of validating system stability and latency under real-world conditions.*

* **Dynamic Realm Handling**: Automatically detects if the current game client matches the required realm; if not, it updates the realm file and launches a fresh client instance.  
* **Intelligent Login & Character Selection**: Reads game states (login screen, character selection) to automatically input credentials and enter the game world with a pre-selected character.  
* **Advanced Anti-AFK System**: Keeps multiple characters online by simulating randomized movement and jumping patterns, preventing inactivity-based disconnections.  
![App Demo](./assets/Animation5.gif)
* **Multi-Threaded Key Spammer**: Enables users to send custom key combinations to specific game windows at user-defined intervals.  
![App Demo](./assets/anim6.gif)
* **P/Invoke & Win32 API**: Leverages `user32.dll` and `kernel32.dll` to perform low-level window management and simulate keyboard input directly to game clients.  
* **High-Concurrency Architecture**: Spawns independent threads for each spam task to ensure non-blocking performance across multiple game instances.  
* **Thread Safety & Synchronization**: Employs **atomic read-modify-write** operations on shared variables to maintain state integrity and prevent race conditions in a multi-threaded environment.  
* **Data Persistence**: Uses a `DataSet` backed by an **XML file system** for lightweight, portable storage of account credentials and configuration data. 

---

## 🏁 Getting Started & Security Linking

To prevent public credential leaks, all live connection parameters and access tokens are completely decoupled from source control. Follow these steps to configure your local keys and securely link your WinForms client to your server app:

### 1. Set Up the Server App Secrets
1. Navigate into the **`WowCloudServer`** project folder.
2. Locate the file named `appsettings.Example.json`.
3. Create a duplicate copy of it in the same directory and name it exactly **`appsettings.json`** (this file is pre-configured in `.gitignore` to never upload to GitHub).
4. Generate a secure, custom alphanumeric passphrase for your API security token and fill out the properties:
   ```json
   {
     "ConnectionStrings": {
       "SupabaseDb": "Host=your-supabase-host-address;Database=postgres;Username=postgres;Password=your-password;"
     },
     "Security": {
       "ApiKey": "YOUR_CHOSEN_SECURE_TOKEN_STRING"
     }
   }
   ```

### 2. Set Up the WinForms Client Key Link
1. Navigate into the **`WowAccountManager`** desktop project folder.
2. Locate the file named `App.Example.config`.
3. Create a duplicate copy of it in the same directory and name it exactly **`App.config`** (this is your local, untracked operational profile).
4. Update the configuration keys to target your server's hosting port and insert the **exact same** API security token string you defined in your server's JSON file:
   ```xml
   <?xml version="1.0" encoding="utf-8" ?>
   <configuration>
     <appSettings>
       <add key="X-API-KEY" value="YOUR_CHOSEN_SECURE_TOKEN_STRING" />
     </appSettings>
   </configuration>
   ```

### 3. Compile and Launch
1. Open `WowAccountManager.sln` inside Visual Studio.
2. Verify that your server's `wwwroot/index.html` file properties are set to **Build Action: Content** and **Copy to Output Directory: Copy if newer** to ensure the live dashboard copies into Release packages.
3. Build and launch your server app.
4. Launch your WinForms application. When the client executes its initialization step, it will read your `App.config`, pass your secret key through the custom HTTP headers layer, pass the validation filter, and activate your real-time synchronization loop.
5. Open a browser tab to `https://localhost:7189/` to view your dark-mode live dashboard panel.

---

> ## ⚠️ Legal Disclaimer
> 
> **This tool is for educational and research purposes only.** 
>
>### Research Methodology
> Visual demonstrations were captured in a controlled environment for the purpose of validating system stability and latency under real-world conditions. 
> ### Use at Your Own Risk
> The author (and any contributors) are NOT responsible for:
> *   **Account Actions:** Any bans, suspensions, or penalties applied to your accounts by game developers or anti-cheat systems (e.g., VAC, BattlEye, Easy Anti-Cheat).
> *   **System Damage:** Any data loss, hardware failure, or system instability caused by the use of this software.
> *   **Legal Consequences:** Any misuse of this tool that violates local laws or third-party Terms of Service.
> 
> ### License
> This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED. 
> 
> ### Third-Party Notices
> This project incorporates bundled code from third parties. For details and full license texts, please see the [THIRD-PARTY-NOTICES](THIRD-PARTY-NOTICES) file.