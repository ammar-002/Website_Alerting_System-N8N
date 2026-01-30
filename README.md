# 📌 Website Monitoring Workflow (n8n)

An automated website monitoring system that checks URLs listed in Google Sheets and sends Email and WhatsApp alerts when issues are detected.

## 🚀 Features

### ⏱ Scheduled Monitoring
- Workflow runs automatically at specified intervals using the Schedule Trigger node
- Configurable monitoring frequency for continuous uptime tracking

### 📄 Read URLs From Google Sheets
- Fetches URLs, reporting emails, and WhatsApp numbers from Google Sheets
- Centralized configuration management
- Easy to update and maintain monitoring targets

### 🌐 HTTP Status Check
- Sends requests to each URL from the sheet
- Captures full response with `fullResponse` and `neverError` modes
- Handles timeouts and SSL certificate issues gracefully

### 🔀 Status-Based Logic
Switch node detects two types of issues:
- **5xx** → Server Crash (Internal Server Errors)
- **4xx** → Client Error (Bad Request, Not Found, etc.)

### ✉️ Email Notifications (Gmail)
- **Server Crash alerts** for 5xx errors
- **Client Error alerts** for 4xx errors
- Detailed information including URL, status code, and timestamp

### 📱 WhatsApp Alerts
- Sends instant notifications to relevant WhatsApp numbers
- Quick mobile alerts for critical issues

## 🧩 Workflow Architecture

```
Schedule Trigger
     ↓
Get row(s) in sheet (Google Sheets)
     ↓
HTTP Request
     ↓
Switch (Status Logic)
 ├── Send message (WhatsApp)
 ├── Server Crash (Gmail)
 └── Client Error (Gmail)
```

## 📋 Workflow Nodes Details

### 1. Schedule Trigger
- Executes the workflow at regular intervals
- Configurable time-based automation

### 2. Get Row(s) in Sheet
Fetches the following data from Google Sheets:
- URL to monitor
- Reporting Email address
- WhatsApp number for alerts

### 3. HTTP Request
Configuration:
- **Full Response**: `true`
- **Never Error**: `true`
- **Timeout**: 10 seconds
 

Sends HTTP requests to each URL and captures response details.

### 4. Switch (Status Logic)
Routes the workflow based on HTTP status codes:
- **Route 1**: `>= 500` → Server Crash
- **Route 2**: `>= 400` → Client Error

### 5. Server Crash (Gmail)
Sends email notification for 5xx errors containing:
- URL
- Status Code
- Status Message
- Checked timestamp

### 6. Client Error (Gmail)
Sends email notification for 4xx errors with structured information.

### 7. WhatsApp Alert If Crash Happens
Sends instant message containing:
- URL
- Status code
- Error details

 
## 🛠️ Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google Sheets API access
- Gmail account for email notifications
- WhatsApp Business API or integration setup

### Configuration Steps

1. **Create Google Sheet**
   - Set up a sheet with columns: URL, Email, WhatsApp Number
   - Add the websites you want to monitor

2. **Configure n8n Credentials**
   - Google Sheets OAuth2 credentials
   - Gmail credentials
   - WhatsApp API credentials

3. **Import Workflow**
   - Import the workflow JSON into your n8n instance
   - Configure all credential connections

4. **Set Schedule**
   - Adjust the Schedule Trigger to your preferred monitoring interval
   - Recommended: Every 5-15 minutes

5. **Test**
   - Run the workflow manually to verify all connections
   - Check email and WhatsApp delivery

## 📊 Google Sheets Format

| URL | Email | WhatsApp Number |
|-----|-------|----------------|
| https://example.com | admin@example.com | +1234567890 |
| https://mysite.com | team@mysite.com | +0987654321 |

## ⚙️ Customization Options

- Modify email templates in Gmail nodes
- Add more status code conditions in the Switch node
- Integrate additional notification channels (Slack, SMS, etc.)
- Add logging or database storage nodes
- Customize check intervals in Schedule Trigger

 

## 📈 Future Enhancements

- [ ] Response time tracking
- [ ] Historical uptime reporting
- [ ] Dashboard integration
- [ ] Custom health check endpoints
- [ ] Incident escalation logic

## 🤝 Contributing

Feel free to submit issues or pull requests to improve this monitoring workflow.

## 📄 License

This workflow is provided as-is for monitoring purposes.

---

**Made with ❤️ using n8n**
