### Tweetcord
https://github.com/Yuuzi261/Tweetcord

Run 
```
python3 bot.py
```

Create .env
```

```

Add notifications in discord
```
/add notifier
```

### Invite bot
https://discord.com/developers/applications

Generate an invite link with this permission integer:

> `2147666944`

Use the Discord Developer Portal:

- Go to your bot's settings → OAuth2 → URL Generator

 Get the bot token

1. Under the **“Bot”** tab, you’ll see **“Token”**.
    
2. Click **“Reset Token”** or **“Copy”** to reveal and copy your token.

---
### Run in background

Start new tmux session
```
tmux new -s tweetcord
```

Detach from Session (Leave it Running)
```
Ctrl + B, then D
```

Reattach to Your Session
```
tmux attach -t tweetcord
```
