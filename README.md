# Automated Confession System

A Flask-based anonymous confession system with automated content moderation, Instagram integration, and device fingerprinting capabilities.

## 🌟 Features

- **Anonymous Confession Submission**: Secure and private confession system
- **Automated Content Moderation**: 
  - Sentiment analysis using Google NLP and VADER
  - Name/mention detection with fuzzy matching
  - Objectionable content filtering via Google Gemini AI
- **Instagram Integration**: Auto-posting of approved confessions with custom templates
- **Advanced Security**:
  - CAPTCHA verification
  - Device fingerprinting (IP-based, cookie-based, canvas fingerprinting)
  - Geolocation-based restrictions (India-only)
  - Rate limiting and cooldown periods
- **Admin Dashboard**: Comprehensive moderation tools
- **Device Management**: Block/unblock suspicious devices
- **Telegram Notifications**: Real-time alerts for new submissions

## 🛠️ Tech Stack

- **Backend**: Flask (Python)
- **Database**: SQLite
- **APIs**: 
  - Google Natural Language API (Sentiment Analysis)
  - Google Gemini AI (Content Moderation)
  - Instagram Graph API (Auto-posting)
  - Telegram Bot API (Notifications)
- **Security**: Flask-Simple-Captcha, Device Fingerprinting
- **Image Generation**: Pillow (PIL)
- **Dependencies**: 
  - Flask, SQLite3, Requests
  - VADER-Sentiment, NLTK
  - Pillow, Unidecode
  - Flask-Simple-Captcha

## 📋 Prerequisites

- Python 3.8+
- Instagram Business Account
- Google Cloud Platform account (for NLP and Gemini AI)
- Telegram Bot (for notifications)

## 🔧 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/AmitKr-GitsHub/igidr-confession-system.git
cd igidr-confession-system
```

### 2. Create Virtual Environment
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install flask sqlite3 requests pillow nltk vadersentiment unidecode flask-simple-captcha instagrapi
```

### 4. Environment Configuration
Create a `.env` file in your project root:

```env
# Admin Configuration
ADMIN_PASSWORD=your_secure_admin_password_here

# Google APIs
GOOGLE_NLP_API_KEY=your_google_nlp_api_key_here
GOOGLE_GENAI_API_KEY=your_gemini_api_key_here

# Instagram Configuration
INSTAGRAM_USERNAME=your_instagram_business_username
INSTAGRAM_PASSWORD=your_instagram_password

# Telegram Notifications
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
TELEGRAM_CHAT_ID=your_telegram_chat_id

# CAPTCHA Security
CAPTCHA_SECRET_KEY=your_long_captcha_secret_key_here
```

### 5. Database Initialization
```bash
python app.py
```
This will automatically:
- Create `confessions.db` and `blocked_devices.db`
- Set up required tables
- Run database migrations
- Add default consented people

## 🚀 Deployment

### Local Development
```bash
python app.py
```
Access at: `http://localhost:7000`

### Production Deployment

#### Option 1: Gunicorn (Recommended for Linux)
```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:7000 app:app
```

#### Option 2: Waitress (Windows)
```bash
pip install waitress
waitress-serve --host=0.0.0.0 --port=7000 app:app
```

#### Option 3: Docker Deployment
Create `Dockerfile`:
```dockerfile
FROM python:3.9-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create necessary directories
RUN mkdir -p templates static

EXPOSE 7000

CMD ["python", "app.py"]
```

Create `requirements.txt`:
```txt
flask==2.3.3
requests==2.31.0
pillow==10.0.1
nltk==3.8.1
vadersentiment==3.3.2
unidecode==1.3.7
flask-simple-captcha==5.2.2
instagrapi==1.16.42
python-dotenv==1.0.0
```

Build and run:
```bash
docker build -t confession-system .
docker run -p 7000:7000 confession-system
```

## 📁 Project Structure

```
igidr-confession-system/
│
├── app.py                            # Main Flask application
├── requirements.txt                  # Python dependencies
├── .env                             # Environment variables (create this)
├── .gitignore                       # Git ignore rules
│
├── templates/                       # HTML templates
│   ├── index.html                   # Main submission page
│   ├── admin_login.html             # Admin authentication
│   ├── admin_dashboard5.html        # Admin moderation panel
│   ├── admin_consented_people.html  # Manage consented people
│   └── admin_blocked_devices2.html  # Device management
│
├── static/                          # Static assets
│   ├── confession_templates/        # Instagram post templates
│   │   ├── Confession_green.PNG
│   │   ├── Confession_Pinkish.PNG
│   │   ├── confession_main.png
│   │   └── Confession_violet.PNG
│   └── styles/                      # CSS stylesheets
│
├── confessions.db                   # Main database (auto-generated)
├── blocked_devices.db               # Blocked devices DB (auto-generated)
└── README.md                        # This file
```

## 🔐 API Configuration

### Google Cloud Setup
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing one
3. Enable the following APIs:
   - Natural Language API
   - Google AI Gemini API
4. Create API credentials
5. Add keys to your `.env` file

### Instagram Setup
1. Convert to Instagram Business Account
2. Create Facebook Developer account
3. Create a new app in [Facebook Developers](https://developers.facebook.com)
4. Add Instagram Basic Display product
5. Configure Instagram Graph API permissions
6. Generate long-lived access token

### Telegram Bot Setup
1. Message [@BotFather](https://t.me/BotFather) on Telegram
2. Use `/newbot` command to create a bot
3. Save the bot token
4. Start conversation with your bot
5. Get your chat ID from `https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates`

## 🎯 Usage Guide

### For End Users

1. **Access the System**
   - Visit the homepage
   - Complete CAPTCHA verification
   - Submit your confession

2. **Submission Rules**
   - Maximum 1 confession every 3 minutes
   - Content is automatically moderated
   - Objectionable content is rejected
   - Personal names require consent

3. **Check Status**
   - Save your confession ID
   - Use `/status/<confession_id>` to check approval status

### For Administrators

1. **Access Admin Panel**
   - Navigate to `/admin`
   - Login with admin password
   - Access moderation dashboard

2. **Moderation Actions**
   - Approve/reject pending confessions
   - Review flagged content
   - Manage consented people list
   - Monitor device activity

3. **Device Management**
   - View blocked devices
   - Block/unblock specific devices
   - Monitor submission patterns

## 🔒 Security Features

### Multi-Layer Protection
1. **CAPTCHA Verification**: Prevents automated submissions
2. **Rate Limiting**: 3-minute cooldown between submissions
3. **Geolocation Filtering**: Accepts submissions only from India
4. **Device Fingerprinting**: Tracks devices using:
   - IP-based IDs
   - Cookie-based IDs
   - Canvas fingerprinting
5. **Content Analysis**: Automated sentiment and objectionable content detection

### Content Moderation
- **Sentiment Analysis**: Flags highly negative content
- **Name Detection**: Identifies personal mentions
- **Similarity Check**: Prevents duplicate submissions
- **Objectionable Content**: AI-powered content filtering

## 📊 Database Schema

### Confessions Table
```sql
CREATE TABLE confessions (
    id TEXT PRIMARY KEY,
    content TEXT NOT NULL,
    sentiment_score REAL,
    sentiment_label TEXT,
    status TEXT DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    posted_at TIMESTAMP,
    review_reason TEXT,
    device_details TEXT,
    submitter_id_ip TEXT,
    submitter_id_cookie TEXT,
    canvas_fingerprint TEXT
);
```

### Blocked Devices Table
```sql
CREATE TABLE blocked_devices (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    submitter_id_ip TEXT,
    submitter_id_cookie TEXT,
    canvas_fingerprint TEXT,
    reason TEXT,
    blocked_by TEXT,
    blocked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT 1
);
```

### Consented People Table
```sql
CREATE TABLE consented_people (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT UNIQUE NOT NULL,
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🔄 API Endpoints

### Public Endpoints
- `GET /` - Homepage with CAPTCHA
- `POST /submit` - Submit confession
- `GET /status/<confession_id>` - Check status
- `GET /refresh-captcha` - Refresh CAPTCHA
- `GET /check-cooldown` - Check user cooldown

### Admin Endpoints
- `GET /admin` - Admin login
- `POST /admin/login` - Authenticate admin
- `GET /admin/dashboard` - Moderation dashboard
- `GET /admin/approve/<id>` - Approve confession
- `GET /admin/reject/<id>` - Reject confession
- `POST /admin/block-device` - Block device
- `GET /admin/unblock-device/<id>` - Unblock device

## 🛠️ Development

### Adding New Features
1. Follow Flask application structure
2. Add database migrations if needed
3. Update templates accordingly
4. Test security implications

### Customizing Templates
- Modify files in `templates/` directory
- Update static assets in `static/`
- Add new confession templates as needed

### Extending Moderation
- Add new sentiment analysis rules
- Modify name detection patterns
- Update objectionable content criteria

## 🐛 Troubleshooting

### Common Issues

1. **Instagram Posting Fails**
   - Check Instagram credentials
   - Verify business account status
   - Ensure proper API permissions

2. **Database Errors**
   - Check file permissions for `.db` files
   - Verify SQLite3 installation
   - Run database migrations

3. **CAPTCHA Not Working**
   - Verify CAPTCHA secret key
   - Check session configuration
   - Ensure proper environment setup

4. **API Rate Limiting**
   - Monitor Google API quotas
   - Check Instagram posting limits
   - Implement retry mechanisms

### Logs and Monitoring
- Application logs in console
- Database operations logged
- Telegram notifications for important events

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-feature`
3. Make your changes and test thoroughly
4. Commit changes: `git commit -am 'Add new feature'`
5. Push to branch: `git push origin feature/new-feature`
6. Submit a Pull Request

### Code Standards
- Follow PEP 8 for Python code
- Use meaningful commit messages
- Document new features
- Test security implications

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This system is designed for educational and responsible use. Developers must ensure compliance with:

- Instagram's Terms of Service
- Data privacy regulations (GDPR, CCPA, etc.)
- Platform-specific content guidelines
- Local laws regarding data collection

## 🆘 Support

- Create an issue on GitHub for bugs
- Check existing documentation
- Contact maintainers for security issues

---

## 📤 GitHub Upload Instructions

### Step 1: Repository Creation
1. Go to [GitHub](https://github.com) and sign in
2. Click "+" → "New repository"
3. Repository name: `automatic_confession_page`
4. Description: "Flask-based anonymous confession system with automated moderation"
5. Choose visibility (public/private)
6. **Do not** initialize with README
7. Click "Create repository"

### Step 2: Local Repository Setup
Create `.gitignore`:
```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
ENV/
env.bak/
venv.bak/

# Database
*.db
*.sqlite3

# Environment variables
.env
.env.local
.env.production

# Logs
*.log
logs/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Instagrapi sessions
ig_session_*.json
```

### Step 3: Initial Commit and Push
```bash
# Initialize Git
git init

# Add files
git add .

# Initial commit
git commit -m "Initial commit: Automatic Confession System

- Flask-based confession platform
- Automated content moderation
- Instagram integration
- Device fingerprinting security
- Admin moderation dashboard
- Telegram notifications"

# Add remote origin
git remote add origin https://github.com/AmitKr-GitsHub/automatic_confession_page.git

# Push to main branch
git branch -M main
git push -u origin main
```

### Step 4: Repository Finalization
1. Add repository topics: `flask python confession-system instagram-api moderation telegram-bot security`
2. Update repository description if needed
3. Configure branch protection rules
4. Set up GitHub Actions for CI/CD if desired



**⭐ If you find this project useful, please give it a star on GitHub!**
```
