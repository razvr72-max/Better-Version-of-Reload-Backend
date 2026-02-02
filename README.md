# Reload Backend - Localhost Setup Guide

![Imgur](https://i.imgur.com/ImIwpRm.png)

Reload Backend is a universal Fortnite private server backend written in [JavaScript](https://en.wikipedia.org/wiki/JavaScript)

Created by [Burlone](https://github.com/burlone0), This is a modded backend, all main backend credits to [Lawin](https://github.com/Lawin0129)

## 🚀 Quick Setup for Localhost (127.0.0.1)

This version is pre-configured for localhost development and testing.

### 📋 Prerequisites
1. **Node.js** (v16 or higher) - [Download Here](https://nodejs.org/en/)
2. **MongoDB** - [Download Here](https://www.mongodb.com/try/download/community)

### 🔧 Configuration Overview

This localhost version uses the following ports:
- **Backend Port**: `3551` (Main API server)
- **Matchmaker Port**: `80` (Matchmaking service)
- **Game Server Port**: `7777` (Actual game servers)
- **Website Port**: `100` (Web interface, if enabled)
- **Caldera Service Port**: `5000` (For newer Fortnite versions)

### 📁 Directory Structure
```
Reload-Backend-main-backup/
├── Config/
│   └── config.json          # Main configuration file
├── routes/                   # API endpoints
├── responses/               # Game content and responses
├── CloudStorage/            # Cloud storage files
├── structs/                 # Backend logic
├── Website/                 # Web interface
├── ssl/                     # SSL certificates
├── tokenManager/            # Token management
├── xmpp/                    # Chat system
└── DiscordBot/              # Discord integration
```

## 🛠️ Step-by-Step Installation

### 1. Install Dependencies
```bash
# Navigate to the backend directory
cd Reload-Backend-main-backup

# Install required packages
npm install
# Or run the included batch file
install_packages.bat
```

### 2. Configure MongoDB
- Make sure MongoDB is running on your system
- The backend automatically connects to: `mongodb://127.0.0.1/Reload`
- No additional configuration needed for basic setup

### 3. Configure Discord Bot (Optional but Recommended)
1. Open `Config/config.json`
2. Replace the bot token with your own Discord bot token
3. Set your Discord ID as a moderator in the `moderators` array
4. Disable by setting `"bUseDiscordBot": false` if not needed

### 4. Start the Backend
```bash
# Using the batch file
start.bat

# Or manually
node index.js
```

### 5. Configure Proxy (Fiddler)
Use this Fiddler script to redirect Fortnite to your localhost:

```javascript
import Fiddler;

class Handlers
{
    static function OnBeforeRequest(oSession: Session) {
        // Redirect Epic Games servers to localhost
        if (oSession.hostname.Contains("epicgames"))
        {
            if (oSession.HTTPMethodIs("CONNECT"))
            {
                oSession["x-replywithtunnel"] = "ServerTunnel";
                return;
            }
            oSession.fullUrl = "http://127.0.0.1:3551" + oSession.PathAndQuery
        }
    }
}
```

### 6. Create Your First Account
1. Join your Discord server with the bot
2. Use the command: `/create {email} {username} {password}`
3. Create a Host Account as well to be able to host games

### 7. Launch Fortnite
1. Start Fortnite with your preferred launcher
2. Make sure proxy is active
3. Login with your created account credentials
4. You should now be connected to your local backend!

## 🎮 Features Available

### Core Features
- ✅ **Account System**: Create and manage accounts
- ✅ **Locker Management**: Full locker customization
- ✅ **Item Shop**: Auto-rotating item shop with Chapter 1-4 cosmetics
- ✅ **Battle Pass**: Chapter 1 Season 2 - Chapter 3 Season 2
- ✅ **Friends System**: Add/remove friends, chat functionality
- ✅ **Matchmaking**: Solo, Duo, and Tournament playlists
- ✅ **Tournaments**: Solo tournament system with rewards (may work in the future)

### Advanced Features
- ✅ **Discord Bot Integration**: Full account management via Discord
- ✅ **Match Rewards**: V-Bucks rewards for matches
- ✅ **Auto Item Shop**: Rotates daily at configured time
- ✅ **HTTPS/SSL Support**: Can be enabled for secure connections
- ✅ **Multiple Game Servers**: Support for multiple server instances
- ✅ **XMPP Chat**: Party chat and messaging system

## 🔧 Configuration Options

### Key Settings in `config.json`:

```json
{
  "port": 3551,                    // Backend main port
  "matchmakerIP": "127.0.0.1:80", // Matchmaker service
  "gameServerIP": [                // Game servers
    "127.0.0.1:7777:playlist_showdownalt_solo",
    "127.0.0.1:7777:playlist_defaultsolo",
    "127.0.0.1:7777:Playlist_ShowdownTournament_Solo",
    "127.0.0.1:7777:Playlist_ShowdownTournament_Duo"
  ],
  "battlepassseason": 9,           // Current season
  "joinable_version": "9.10",      // Client version
  "bEnableBattlepass": true,       // Enable battle pass
  "bUseAutoRotate": true,          // Auto item shop rotation
  "bRotateTime": "15:30"          // Shop rotation time (UTC)
}
```

## 🎯 Tournament System

The backend includes a fully functional tournament system:

### Available Tournaments:
- **Solo Tournament**: `Playlist_ShowdownTournament_Solo`
- **Duo Tournament**: `Playlist_ShowdownTournament_Duo`

### Tournament Features:
- Automatic scoring system
- Leaderboard integration
- Discord notifications
- Custom tournament codes

## 📱 Discord Bot Commands

### User Commands:
- `/create {email} {username} {password}` - Create account
- `/details` - Get account info
- `/lookup {username}` - Lookup another user
- `/exchange-code` - Generate login code
- `/vbucksamount` - Check V-Bucks balance
- `/claimvbucks` - Claim daily V-Bucks

### Admin Commands:
- `/addall {user}` - Give all cosmetics
- `/addvbucks {user} {amount}` - Add V-Bucks
- `/ban {user}` - Ban user
- `/kick {user}` - Kick user from session
- `/create-custom-match-code {code} {ip} {port}` - Custom match

## 🔒 Security Notes

### Important Security Reminders:
1. **Never share your Discord bot token**
2. **Keep MongoDB secured with authentication in production**
3. **Use HTTPS/SSL for public deployments**
4. **Regularly update dependencies**
5. **Monitor backend logs for suspicious activity**

### Default Security Settings:
- Global chat disabled by default
- Report system disabled
- Cross-banning disabled
- Auto-restart disabled

## 🚨 Troubleshooting

### Common Issues:

#### Backend Won't Start:
- Check if Node.js is installed correctly
- Verify MongoDB is running
- Check port 3551 isn't already in use
- Review error logs in console

#### Can't Connect to Game:
- Ensure Fiddler proxy is configured correctly
- Check if Fortnite is using the proxy
- Verify backend is running
- Try restarting both backend and game

#### Account Creation Fails:
- Check Discord bot token is valid
- Verify you have moderator permissions
- Check Discord bot is online and responding

#### Matchmaking Not Working:
- Verify game server ports are configured correctly
- Check if game servers are actually running
- Review matchmaker logs for errors

### Port Conflicts:
If you experience port conflicts, you can change them in `config.json`:
```json
{
  "port": 3551,              // Change backend port
  "matchmakerIP": "127.0.0.1:19212", // Change matchmaker port
  "gameServerIP": ["127.0.0.1:19151:..."] // Change game server port
}
```

## 🌐 Network Configuration

### For Local Testing:
All services are configured for `127.0.0.1` (localhost)

### For Public Deployment:
1. Replace `127.0.0.1` with your public IP address
2. Configure firewall rules for all ports
3. Set up SSL certificates
4. Update Discord bot redirect URIs

### Port Forwarding (if needed):
- **3551**: Backend API
- **80**: Matchmaker
- **7777**: Game servers
- **100**: Website (if enabled)
- **5000**: Caldera service (if enabled)

## 📊 Performance Tips

### Optimize for Better Performance:
1. **Enable MongoDB indexing** for faster queries
2. **Use SSD storage** for database
3. **Limit concurrent connections** in config
4. **Enable debug logs only when needed**
5. **Regular database cleanup**

### Resource Usage:
- **RAM**: ~200-500MB (varies with player count)
- **CPU**: Low usage for <50 players
- **Disk**: ~2GB for full installation
- **Network**: ~1MB/s per 10 active players

## 🔄 Updates and Maintenance

### Regular Maintenance Tasks:
1. **Backup MongoDB database** regularly
2. **Update Node.js dependencies** monthly
3. **Monitor Discord bot status**
4. **Check item shop rotation** working correctly
5. **Review tournament performance**

### Updating Backend:
1. **Backup current installation**
2. **Download new version**
3. **Copy config files** from backup
4. **Run database migrations** if needed
5. **Test all features** before going live

## 📞 Support

### Getting Help:
- **Discord**: Contact `@astro.dev0` for support
- **GitHub**: Create issues for bug reports
- **Documentation**: Check this README first

### Reporting Bugs:
1. **Include error logs**
2. **Describe reproduction steps**
3. **Specify your configuration**
4. **Mention Fortnite version** used

---

## Credits to burlone even though he didnt help me with this but he made most of the stuff thats in this backend

