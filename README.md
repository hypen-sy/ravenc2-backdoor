# ravenc2-backdoor
This is created by South Korean middle school students
**RavenC2** is a Remote Administration Tool (RAT) that leverages the Discord API as a Command & Control (C2) server. This project was created for the purpose of researching and learning the offensive side of cybersecurity, including attack vectors, malware behavior, and C2 communication channel evasion.

>  **WARNING: This project is intended for educational and ethical research purposes only.**
>
> Using this tool on unauthorized systems is strictly illegal and can lead to severe legal consequences. The developer assumes no liability and is not responsible for any misuse or damage caused by this tool. Use only in your own test environments or on systems for which you have explicit permission.

---

##  Features

- ** Remote Shell**: Execute any shell command on the victim's system and receive the output.
- ** Keylogging**: Captures all keyboard inputs from the user, logging which application was active at the time.
- ** Screenshot**: Takes a screenshot of the victim's current screen and sends it back as an image file.
- ** File System Control**:
  - **File Download**: Exfiltrate any file from the victim's PC.
  - **Directory Traversal**: Navigate the remote file system freely with `cd` and `pwd` commands.
- ** System Information**: Gathers detailed system information, including OS, username, IP address, and hardware specs.
- ** Persistence**: Achieves persistence by obtaining administrator privileges and copying itself to the Windows Startup folder, ensuring it runs automatically on reboot.
- ** Ransomware Module**: Includes functionality to encrypt/decrypt files in a specified path using AES-256 (for research purposes).
- ** Dropper**: Provides a dropper with a fake 'Windows Update' GUI to deceive the user into installing the backdoor.

---

##  Architecture (How it Works)

RavenC2 uses Discord channels as a communication relay, eliminating the need for a central server.

1.  **Victim**: When `victim_discord.py` is executed, it connects to a specified Discord channel and "checks in" by sending its unique ID (`IP:UUID`).
2.  **Attacker**: Running `attacker_discord.py` opens a console that monitors the C2 channel.
3.  **Command**: The attacker selects a specific victim from the list and enters a command (e.g., `pwd`, `ipconfig`).
4.  **Communication**: The attacker bot sends a message to the channel formatted as `[Victim_ID] [Command]`.
5.  **Execution & Report**: All victim bots monitor the channel. When a victim bot sees a message starting with its own ID, it executes the command and posts the result back to the channel.

---

## ⚙️ Setup

#### Prerequisites
- Python 3.8+
- Two Discord accounts (one for the attacker, one for the victim)

#### 1. Discord Bot Setup
1.  Go to the [Discord Developer Portal](https://discord.com/developers/applications).
2.  Click `New Application` and create **two separate applications** (e.g., `attacker-bot`, `victim-bot`).
3.  For each application, navigate to the **Bot** tab and enable the following Privileged Gateway Intents:
    - **SERVER MEMBERS INTENT**
    - **MESSAGE CONTENT INTENT**
4.  Copy the bot token (`BOT_TOKEN`) for each bot and store it securely.
5.  Go to the **OAuth2 -> URL Generator** tab. Select the `bot` and `applications.commands` scopes. Under "Bot Permissions," select `Administrator` to generate an invite link.
6.  Use the generated links to invite both bots to your Discord server.

#### 2. Source Code Configuration
1.  Clone or download this repository.
2.  Open the `attacker_discord.py` and `victim_discord.py` files.
3.  In your Discord server, right-click on the channel you want to use for C2 and select **'Copy Channel ID'**.
4.  Update the following variables in both files with your own information:
    ```python
    # Modify in both attacker_discord.py and victim_discord.py
    BOT_TOKEN = "YOUR_UNIQUE_BOT_TOKEN_HERE"
    C2_CHANNEL_ID = 123456789012345678 # Paste the copied Channel ID here
    ```

#### 3. Install Dependencies
Open a terminal and run the following command to install all required libraries:
```bash
pip install -r requirements.txt
```

---

##  Usage

1.  **Run the Victim Program**: On the Windows PC you want to test, run `victim_discord.py` (or a PyInstaller-built executable).
2.  **Run the Attacker Console**: On your own machine, run `attacker_discord.py`.
    ```bash
    python attacker_discord.py
    ```
3.  The console will start, and after a moment, the victim will check in and appear on the list.

#### Key Commands
- `list` or `ls`: View the list of currently connected victims.
- `refresh`: Force a refresh of the victim list.
- `<victim_id>`: Select a victim to control by entering their ID (e.g., `58.238.63.201:e769...`).
- `back`: Deselect the currently selected victim.
- `pwd`, `ipconfig`, `screenshot`, etc.: With a victim selected, enter any command you wish to execute.

---
## ⚖️ License
This project is licensed under the MIT License. See the `LICENSE` file for details.
