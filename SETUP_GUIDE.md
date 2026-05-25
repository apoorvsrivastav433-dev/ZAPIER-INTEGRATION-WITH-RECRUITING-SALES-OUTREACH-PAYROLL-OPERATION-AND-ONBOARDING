# Sales Pipeline Manager - Complete Setup Guide

## 🚀 Quick Start

### Step 1: Open in VS Code Live Server
1. Download the `sales-pipeline-manager.html` file
2. Open it in VS Code
3. Right-click → "Open with Live Server"
4. The app will open at `http://localhost:5500`

## 📋 Features Overview

### Dashboard
- **Real-time metrics**: Total leads, conversion rate, follow-ups due, overdue tasks
- **Pipeline overview**: Visual funnel showing lead progression
- **Recent activity**: Track all interactions and outcomes
- **Charts**: Lead source distribution with interactive visualization

### Leads Management
- **Lead database**: View all leads with their email and stage
- **Quick actions**: Email and task creation buttons
- **Search functionality**: Find leads by name or email
- **Add new leads**: Modal form to quickly add leads

### Automations (Zapier Integration)
- **6 pre-built workflows**: Ready-to-activate automations
- **One-click activation**: Enable workflows instantly
- **Status tracking**: See which workflows are active

### Analytics
- **Revenue metrics**: Monthly sales performance
- **Pipeline analysis**: Breakdown by stage
- **Conversion funnel**: Lead-to-close visualization
- **Key metrics**: Average deal size, sales cycle, win rate

### Settings
- **Integration status**: View connected CRM, email, tasks, and chat tools
- **Zapier webhook**: Configure API endpoint
- **Sync frequency**: Choose real-time or scheduled sync

---

## 🔌 Zapier Integration Setup

### Step 1: Create Zapier Account
1. Go to [zapier.com](https://zapier.com)
2. Sign up or log in
3. Go to "Zaps" → "Create Zap"

### Step 2: Set Up Workflow #1 - HubSpot Lead Sync

**Trigger: HubSpot - New Contact**
```
App: HubSpot
Trigger: New Contact
Choose Account: [Select your HubSpot account]
Contact Property: All
```

**Action: Webhook - Post to URL**
```
App: Webhooks by Zapier
Action: POST
URL: Your Webhook URL (see Settings tab)
Payload Type: JSON
Data:
{
  "action": "new_lead",
  "name": "First Name Last Name",
  "email": "Email",
  "company": "Company",
  "phone": "Phone"
}
```

**Action: Slack - Send Message**
```
App: Slack
Action: Send Message
Channel: #sales
Message: "🎉 New Lead: {{name}} from {{company}} ({{email}})"
```

---

### Step 3: Set Up Workflow #2 - Email Campaign

**Trigger: Manual (or use a date trigger)**
```
App: Schedule by Zapier
Trigger: Every Day at [Time]
```

**Action: Gmail - Send Email**
```
App: Gmail
Action: Send Email
To: [Lead Email]
Subject: [Template or custom]
Body: [Follow-up message]
```

**Action: HubSpot - Create or Update Contact**
```
App: HubSpot
Field: Last Contacted (Date)
Value: Today
```

---

### Step 4: Set Up Workflow #3 - Auto Task Creation

**Trigger: HubSpot - Contact Enters Stage**
```
App: HubSpot
Trigger: Contact Enters Stage
Stage: [Choose a stage like "Qualified"]
```

**Action: Asana - Create Task**
```
App: Asana
Action: Create Task
Project: [Your Sales Project]
Name: Follow-up with {{name}}
Due Date: In 3 days
Assignee: You
```

**Action: Slack - Send Message**
```
Message: "📋 Task created: Follow-up with {{name}}"
```

---

### Step 5: Set Up Workflow #4 - Slack Notifications

**Trigger: HubSpot - Contact Changed**
```
App: HubSpot
Trigger: Contact Changed
Field: Pipeline (or Stage)
Changed to: [High-value stage]
```

**Action: Slack - Send Message**
```
App: Slack
Channel: #sales-alerts
Message: "🔥 ALERT: {{name}} moved to {{Pipeline}}!
Company: {{Company}}
Email: {{Email}}"
```

---

### Step 6: Set Up Workflow #5 - Lead Scoring

**Trigger: Gmail - New Email from Address**
```
App: Gmail
Trigger: New Email
From Address: [Your sales email]
```

**Filter: Gmail Label Contains**
```
Important labels or flagged emails
```

**Action: HubSpot - Update Contact**
```
App: HubSpot
Field: Score (Custom Field)
Value: Increment by 10 points
```

---

### Step 7: Set Up Workflow #6 - Daily Digest

**Trigger: Schedule by Zapier**
```
Trigger: Every Day at 9:00 AM
```

**Action: Gmail - Send Email**
```
To: your-email@company.com
Subject: Daily Sales Pipeline Update
Body: [See template below]
```

**Template:**
```
Daily Pipeline Summary - [Date]

Today's Activity:
- New Leads: [Count]
- Emails Sent: [Count]
- Meetings Scheduled: [Count]
- Deals Closed: [Count]

Pipeline Status:
- Leads: 12
- Qualified: 8
- Negotiation: 3
- Closed: 1

Action Items:
- Follow-ups needed: 7
- Overdue tasks: 3

Have a great day! 🚀
```

---

## 🔗 CRM Connections

### HubSpot Setup

1. **Get Your API Key**
   - Go to HubSpot → Settings → Integrations → API Keys
   - Create a new key or copy existing one

2. **Connect to Zapier**
   - In Zapier, click "Connect" for HubSpot
   - Choose "HubSpot"
   - Authenticate with your HubSpot account
   - Grant permissions for Contacts, Companies, and Deals

3. **Verify Connection**
   - Test the connection in your Settings tab
   - Should see "Connected as: [email]"

### Gmail Setup

1. **Connect to Zapier**
   - Choose "Gmail" in Zapier
   - Log in with your Gmail account
   - Grant permission to send emails

2. **Email Tracking** (Optional)
   - Use Gmail labels to track outreach
   - Create labels: "Sales Follow-up", "Meeting Scheduled"
   - Zapier can trigger on these labels

### Asana Setup

1. **Create a Sales Project**
   - Go to Asana → New Project
   - Name: "Sales Follow-ups"
   - Template: List or Timeline

2. **Connect to Zapier**
   - Choose "Asana" in Zapier
   - Log in with your Asana account
   - Select the Sales project

### Slack Setup

1. **Create #sales Channel**
   - In Slack workspace → Create Channel → #sales
   - Create #sales-alerts for high-priority notifications

2. **Connect to Zapier**
   - Choose "Slack" in Zapier
   - Select your workspace
   - Authorize Zapier app
   - Grant message sending permissions

---

## 📊 Workflow Activation Checklist

- [ ] HubSpot Lead Sync (New contacts → Slack notification)
- [ ] Email Campaign (Scheduled emails with tracking)
- [ ] Auto Task Creation (Stage change → Asana task)
- [ ] Slack Notifications (Deal updates → Slack alerts)
- [ ] Lead Scoring (Email activity → HubSpot score)
- [ ] Daily Digest (Morning summary email)

---

## 🎯 Usage Tips

### For Sales Teams
1. **Morning Routine**: Check the Daily Digest email
2. **Lead Review**: Open the Leads tab to see new additions
3. **Quick Actions**: Use Email and Task buttons for instant follow-up
4. **Dashboard Check**: Monitor pipeline metrics in real-time

### For Managers
1. **Analytics Tab**: Monitor team performance and pipeline health
2. **Automation Settings**: Ensure all workflows are active
3. **Integration Status**: Verify all tools are connected
4. **Weekly Reports**: Use data to identify bottlenecks

### Best Practices
- **Set up automation first** before adding manual data
- **Use consistent naming** across HubSpot, Gmail, Asana, Slack
- **Review workflows monthly** to optimize for your process
- **Test each workflow** before going live with team
- **Monitor alerts** in Slack daily for high-priority leads

---

## 🔧 Customization

### Modify Webhook Data
In Settings tab, you can:
- Change sync frequency
- Add custom fields
- Exclude certain lead types
- Set up conditional routing

### Create Custom Workflows
1. Click "Create Zap" in Zapier
2. Choose triggers from: HubSpot, Gmail, Asana, Slack
3. Add actions you need
4. Test before activating
5. Give it a meaningful name

### Add More Integrations
Support for:
- Salesforce
- Airtable
- Microsoft Teams
- Google Sheets
- Outlook
- Trello
- ClickUp

---

## 📞 Troubleshooting

### "Connection Failed" Error
- [ ] Check internet connection
- [ ] Verify API key in HubSpot Settings
- [ ] Re-authenticate in Zapier
- [ ] Check Zapier account is active

### Workflows Not Triggering
- [ ] Verify trigger condition (new contact, stage change, etc.)
- [ ] Check Zapier account has enough tasks
- [ ] Test the zap manually in Zapier
- [ ] Check email formatting in actions

### Data Not Syncing
- [ ] Verify webhook URL in Settings
- [ ] Check HubSpot fields match workflow
- [ ] Review Zapier logs for errors
- [ ] Restart Live Server in VS Code

### Email Not Sending
- [ ] Gmail account connected in Zapier
- [ ] Allow less secure app access in Gmail settings
- [ ] Check recipient email address is valid
- [ ] Verify Gmail not blocking Zapier

---

## 📈 Next Steps

1. **Phase 1**: Set up HubSpot sync and email campaign
2. **Phase 2**: Add Slack notifications and daily digest
3. **Phase 3**: Implement task automation in Asana
4. **Phase 4**: Add lead scoring and advanced filters

---

## 💡 Advanced Features

### Conditional Logic
Create workflows that trigger based on:
- Lead score > 50
- Company size > 100 employees
- Industry = Technology
- Last contacted > 30 days ago

### Multi-step Sequences
1. New lead detected
2. Auto-send intro email
3. Wait 48 hours
4. Send follow-up email
5. Create Asana task if no response

### Team Routing
Route leads based on:
- Sales rep assignment
- Territory
- Product category
- Deal size

---

## 📚 Resources

- [Zapier Documentation](https://zapier.com/help)
- [HubSpot API Reference](https://developers.hubspot.com)
- [Gmail API](https://developers.google.com/gmail/api)
- [Asana API](https://developers.asana.com)
- [Slack API](https://api.slack.com)

---

## 🆘 Support

If you encounter issues:

1. **Check the browser console** (F12 → Console tab)
2. **Review Zapier logs** (Zaps → Click zap → Zap history)
3. **Verify API connections** (Settings tab)
4. **Test each workflow** independently

---

## 📝 Version

**Sales Pipeline Manager v1.0**
- Compatible with: Chrome, Firefox, Safari, Edge
- Responsive: Desktop, Tablet, Mobile
- Last Updated: May 2026

---

Enjoy your automated sales pipeline! 🚀
