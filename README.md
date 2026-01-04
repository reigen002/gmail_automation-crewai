# Gmail Automation with CrewAI 📧✨

Gmail Automation with CrewAI is an intelligent email management system that uses AI agents to categorize, organize, respond to, and clean up your Gmail inbox automatically.

## ✨ Features

![Status](https://img.shields.io/badge/Status-Active-brightgreen)

- **📋 Email Categorization**: Automatically categorizes emails into specific types (newsletters, promotions, personal, etc.)
- **🔔 Priority Assignment**: Assigns priority levels (HIGH, MEDIUM, LOW) based on content and sender with strict classification rules
- **🏷️ Smart Organization**: Applies Gmail labels and stars based on categories and priorities
- **💬 Automated Responses**: Generates draft responses for important emails that need replies
- **📱 Slack Notifications**: Sends creative notifications for high-priority emails
- **🧹 Intelligent Cleanup**: Safely deletes low-priority emails based on age and category
- **🎬 YouTube Content Protection**: Special handling for YouTube-related emails
- **🗑️ Trash Management**: Automatically empties trash to free up storage space
- **🧵 Thread Awareness**: Recognizes and properly handles email threads

## 🚀 Installation

```bash
# Clone the repository
git clone https://github.com/reigen002/gmail_automation-crewai.git
cd gmail_automation-crewai

# Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
crewai install
```

## ⚙️ Configuration

### Step 1: Create Environment File

Create a `.env` file in the root directory of the project with the following structure:

```env
# LLM Provider Configuration
MODEL=openai/gpt-4o-mini
OPENAI_API_KEY=your_openai_api_key_here

# Gmail Credentials
EMAIL_ADDRESS=your_email@gmail.com
APP_PASSWORD=your_16_character_app_password

# Slack Notifications (Optional)
SLACK_WEBHOOK_URL=your_slack_webhook_url_here
```

### Step 2: Set Up Gmail App Password 🔑

**Important**: You must have 2-Step Verification enabled on your Google Account to create app passwords.

#### Enable 2-Step Verification (if not already enabled)

1. Visit [Google Account Security](https://myaccount.google.com/security)
2. Click on **2-Step Verification** under "Signing in to Google"
3. Follow the prompts to set up 2-Step Verification using your phone number or authenticator app
4. Complete the verification process

#### Create Gmail App Password

1. Go to [Google Account Settings](https://myaccount.google.com/)
2. Click on **Security** in the left sidebar
3. Under "Signing in to Google", click **2-Step Verification**
4. Scroll down to the bottom of the page
5. Click on **App passwords** (you may need to verify your password again)
6. In the "Select app" dropdown, choose **Mail**
7. In the "Select device" dropdown, choose **Other (Custom name)**
8. Type `Gmail CrewAI Automation` as the custom name
9. Click **Generate**
10. A 16-character password will appear (format: `xxxx xxxx xxxx xxxx`)
11. **Copy this password immediately** (you won't be able to see it again)
12. Paste it in your `.env` file as `APP_PASSWORD` (remove spaces)
13. Click **Done**

**Example:**
```env
EMAIL_ADDRESS=john.doe@gmail.com
APP_PASSWORD=abcdabcdabcdabcd
```

**⚠️ Security Notes:**
- Never share your app password with anyone
- Store it securely in your `.env` file
- Add `.env` to your `.gitignore` file
- If compromised, revoke it immediately from [App passwords settings](https://myaccount.google.com/apppasswords)

### Step 3: Set Up Slack Webhook (Optional) 🔗

Slack notifications will alert you when high-priority emails arrive.

#### Create a Slack App

1. Go to [Slack API Apps](https://api.slack.com/apps)
2. Click **Create New App**
3. Select **From scratch**
4. Enter app name: `Gmail Automation Notifications`
5. Select your Slack workspace from the dropdown
6. Click **Create App**

#### Enable Incoming Webhooks

1. On the app settings page, find **Incoming Webhooks** in the left sidebar under "Features"
2. Toggle the switch to **ON** (Activate Incoming Webhooks)
3. Scroll down and click **Add New Webhook to Workspace**
4. Select the channel where you want notifications (e.g., `#email-alerts` or `#general`)
5. Click **Allow**

#### Copy Webhook URL

1. You'll be redirected back to the Incoming Webhooks page
2. Find the **Webhook URL** section
3. Copy the complete URL (starts with `https://hooks.slack.com/services/`)
4. Paste it in your `.env` file as `SLACK_WEBHOOK_URL`

**Example:**
```env
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX
```

#### Customize Your Slack App (Optional)

1. Go to **Basic Information** in the left sidebar
2. Scroll down to **Display Information**
3. Add an **App Icon** (e.g., 📧 emoji or custom image)
4. Update the **Description**: "AI-powered email notification system"
5. Choose a background color
6. Click **Save Changes**

#### Test the Webhook

You can test your webhook with this curl command:

```bash
curl -X POST -H 'Content-type: application/json' \
--data '{"text":"🎉 Gmail Automation is connected to Slack!"}' \
YOUR_WEBHOOK_URL
```

Replace `YOUR_WEBHOOK_URL` with your actual webhook URL.

### Step 4: Configure LLM Provider

Choose one of the following LLM providers:

#### Option 1: OpenAI (Recommended)

1. Go to [OpenAI API Keys](https://platform.openai.com/api-keys)
2. Click **Create new secret key**
3. Name it `Gmail CrewAI`
4. Copy the key and paste it in your `.env` file

```env
MODEL=openai/gpt-4o-mini
OPENAI_API_KEY=sk-proj-...your-key-here
```

#### Option 2: Google Gemini

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click **Get API key**
3. Create a new API key or use existing one
4. Copy and paste in your `.env` file

```env
MODEL=gemini/gemini-2.0-flash
GEMINI_API_KEY=your-gemini-api-key-here
```

#### Option 3: Ollama (Local)

1. Install Ollama from [ollama.com](https://ollama.com/)
2. Pull a model with tool-calling capabilities:

```bash
ollama pull llama3-groq-tool-use
```

3. Update your `.env` file:

```env
MODEL=ollama/llama3-groq-tool-use
```

**Note**: Ollama models may have compatibility issues with complex tool calling.

### Verify Your Configuration

Your final `.env` file should look like this:

```env
# LLM Provider
MODEL=openai/gpt-4o-mini
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxx

# Gmail Credentials
EMAIL_ADDRESS=yourname@gmail.com
APP_PASSWORD=abcdabcdabcdabcd

# Slack Webhook (Optional)
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX
```

**✅ Configuration Complete!** You're ready to run the application.

## 📧 How It Works

This application uses the IMAP (Internet Message Access Protocol) to securely connect to your Gmail account and manage your emails.

### 🔄 IMAP Connection Process

1. **Secure Connection**: The application establishes a secure SSL connection to Gmail's IMAP server (`imap.gmail.com`)

2. **Authentication**: It authenticates using your email address and app password (not your regular Google password)

3. **Mailbox Access**: Once authenticated, it can access your inbox and other mailboxes to:
   - Read unread emails
   - Apply labels
   - Move emails to trash
   - Save draft responses

4. **Safe Disconnection**: After each operation, the connection is properly closed to maintain security

IMAP allows the application to work with your emails while they remain on Google's servers, unlike POP3 which would download them to your device. This means you can still access all emails through the regular Gmail interface.

**Security Note**: Your credentials are only stored locally in your `.env` file and are never shared with any external services.

## 🔍 Usage

Run the application with:

```bash
crewai run
```

You'll be prompted to enter the number of emails to process (default is 5).

The application will:
1. 📥 Fetch your unread emails
2. 🔎 Categorize them by type and priority
3. ⭐ Apply appropriate labels and stars
4. ✏️ Generate draft responses for important emails
5. 🔔 Send Slack notifications for high-priority items
6. 🗑️ Clean up low-priority emails based on age
7. 🧹 Empty the trash to free up storage space

## 🌟 Special Features

- **📅 Smart Deletion Rules**:
  - Promotions older than 2 days are automatically deleted
  - Newsletters older than 7 days (unless HIGH priority) are deleted
  - Shutterfly emails are always deleted regardless of age
  - Receipts and important documents are archived instead of deleted

- **🎬 YouTube Protection**: All YouTube-related emails are preserved and marked as READ_ONLY (you'll respond directly on YouTube)

- **✍️ Smart Response Generation**: Responses are tailored to the email context and include proper formatting

- **💡 Creative Slack Notifications**: Fun, attention-grabbing notifications for important emails

- **🧵 Thread Handling**: Properly tracks and manages email threads to maintain conversation context

## 🛠️ Architecture

The system uses 5 specialized AI agents working in sequence:

1. **Email Categorizer** - Analyzes and categorizes incoming emails
2. **Email Organizer** - Applies labels and stars based on categories
3. **Response Generator** - Creates draft responses for important emails
4. **Notification Agent** - Sends Slack alerts for high-priority items
5. **Cleanup Agent** - Safely removes old promotional content

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📚 References

- [CrewAI](https://github.com/crewAIInc/crewAI/)
- [IMAP Protocol for Gmail](https://support.google.com/mail/answer/7126229)
- [Slack API](https://api.slack.com/messaging/webhooks)
- [Ollama](https://ollama.com/library)
- [Gemini](https://ai.google.dev/gemini-api)
- [OpenAI](https://openai.com/api/)

