# Advanced Zapier Workflows & Webhook Guide

## 🔌 Webhook Payload Examples

### 1. New Lead from HubSpot

**Trigger**: New Contact in HubSpot
**Webhook Payload**:

```json
{
  "action": "new_lead",
  "timestamp": "2024-05-25T10:30:00Z",
  "lead": {
    "id": "hubspot_contact_123",
    "name": "Emma Chen",
    "email": "emma.chen@techcorp.com",
    "company": "Tech Corporation",
    "phone": "+1-555-0101",
    "source": "LinkedIn",
    "industry": "Technology",
    "companySize": "500-1000",
    "score": 0,
    "stage": "lead",
    "createdDate": "2024-05-25T10:30:00Z",
    "properties": {
      "lifecycleStage": "subscriber",
      "ownerId": "sales_rep_001",
      "notes": ""
    }
  },
  "actions": [
    "send_slack_notification",
    "create_crm_record",
    "log_activity"
  ]
}
```

**Zapier Actions to Follow**:
1. Send to HubSpot - Create/Update Contact
2. Send Slack message to #sales-alerts
3. Create Asana task for follow-up

---

### 2. Lead Stage Change

**Trigger**: Contact Moves to Qualified Stage
**Webhook Payload**:

```json
{
  "action": "stage_change",
  "timestamp": "2024-05-25T14:20:00Z",
  "lead": {
    "id": "hubspot_contact_456",
    "name": "James Brown",
    "email": "james.brown@startup.io",
    "previousStage": "lead",
    "newStage": "qualified",
    "stageDuration": "3 days",
    "company": "Startup Inc",
    "dealValue": "$50,000"
  },
  "trigger": {
    "field": "dealstage",
    "oldValue": "lead",
    "newValue": "qualified",
    "changedAt": "2024-05-25T14:20:00Z"
  },
  "actions": [
    "send_slack_alert",
    "create_task",
    "send_email_template"
  ]
}
```

**Zapier Actions to Follow**:
1. Slack: Send high-priority alert with deal details
2. Asana: Create "Demo Call" task due in 2 days
3. Gmail: Send templated follow-up email

---

### 3. Email Engagement Tracking

**Trigger**: Email Opened/Clicked (via Gmail)
**Webhook Payload**:

```json
{
  "action": "email_engagement",
  "timestamp": "2024-05-25T16:45:00Z",
  "engagement": {
    "type": "opened",
    "leadId": "hubspot_contact_789",
    "leadName": "Sarah Kim",
    "leadEmail": "sarah.kim@enterprise.com",
    "emailSubject": "Custom Product Demo",
    "emailSentAt": "2024-05-25T09:00:00Z",
    "emailOpenedAt": "2024-05-25T16:45:00Z",
    "timeSinceOpen": "7.75 hours",
    "emailTemplate": "follow_up_sequence_1"
  },
  "scoring": {
    "currentScore": 35,
    "scoreIncrement": 10,
    "newScore": 45,
    "scoringReason": "Email engagement"
  },
  "actions": [
    "update_lead_score",
    "trigger_follow_up",
    "log_activity"
  ]
}
```

**Zapier Actions to Follow**:
1. HubSpot: Update lead score (+10 points)
2. Delay 48 hours
3. Gmail: Send follow-up email if no response
4. Slack: Notify if score reaches 50+

---

## 🤖 Complete Workflow Templates

### Workflow 1: Inbound Lead Nurture Sequence

```
TRIGGER: HubSpot - New Contact
│
├─ CHECK: Score < 25?
│  │
│  └─ YES
│     ├─ Delay: 1 hour
│     ├─ GMAIL: Send Welcome Email
│     │  Subject: "Welcome to [Company]"
│     │  Body: "Hi {{firstName}}, thanks for signing up..."
│     │
│     ├─ Delay: 24 hours
│     ├─ CHECK: Email Opened?
│     │  │
│     │  ├─ YES: Score +15, send case study
│     │  └─ NO: Send open reminder
│     │
│     ├─ Delay: 48 hours
│     ├─ ASANA: Create task "Follow-up with {{name}}"
│     │  Due: 3 days
│     │  Priority: Medium
│     │
│     └─ SLACK: Post to #sales
│        "📌 {{name}} from {{company}} needs follow-up"
│
└─ END
```

---

### Workflow 2: Deal Progression Alert System

```
TRIGGER: HubSpot - Contact Stage Changed
│
├─ CHECK: New Stage = "Qualified"?
│  │
│  └─ YES
│     ├─ SLACK: Send Alert
│     │  Channel: #sales-alerts
│     │  Message: "🎯 QUALIFIED: {{name}} ({{company}})"
│     │  Deal Value: {{dealValue}}
│     │
│     ├─ ASANA: Create Project (if doesn't exist)
│     │  Name: "Deal - {{name}} - {{company}}"
│     │
│     └─ ASANA: Create Task
│        Title: "Schedule Demo with {{name}}"
│        Due: 1 day
│        Assignee: {{salesRep}}
│
├─ CHECK: New Stage = "Negotiation"?
│  │
│  └─ YES
│     ├─ SLACK: Post Deal Update
│     │  "💼 {{name}} entered Negotiation stage"
│     │  "Deal Value: {{dealValue}}"
│     │
│     ├─ GMAIL: Send Negotiation Email
│     │  Subject: "Let's discuss the details"
│     │
│     └─ ASANA: Create Checklist
│        ├─ Prepare quote
│        ├─ Review contract
│        ├─ Get legal approval
│        └─ Schedule call
│
└─ CHECK: New Stage = "Closed"?
   │
   └─ YES
      ├─ SLACK: Announce Win
      │  "🎉 CLOSED WON: {{name}}!"
      │  "{{dealValue}} deal finalized"
      │
      ├─ GMAIL: Send Confirmation Email
      │  "Thank you for choosing us!"
      │
      └─ ASANA: Move to "Onboarding" project
```

---

### Workflow 3: Smart Lead Scoring System

```
TRIGGER: Any CRM Activity (Contact Changed)
│
├─ SCORE +10: Email Opened
├─ SCORE +15: Link Clicked
├─ SCORE +20: Demo Attended
├─ SCORE +25: Proposal Sent
├─ SCORE +30: Demo Request
│
├─ SCORE -5: Email Bounced
├─ SCORE -10: No Activity (30 days)
│
├─ ACTION: Score >= 50?
│  │
│  ├─ YES: Move to "Sales Ready" stage
│  │
│  └─ SLACK: Alert Sales Team
│     "⭐ HIGH-VALUE LEAD: {{name}}"
│     "Score: {{score}}/100"
│     "Recommended Action: Schedule Call"
│
└─ ACTION: Score < 25?
   │
   └─ YES: Move to "Nurture" campaign
      MAILCHIMP: Add to nurture sequence
      GMAIL: Auto-send content series
```

---

### Workflow 4: Daily Pipeline Management Digest

```
TRIGGER: Schedule - Every Day at 9:00 AM
│
├─ GET: HubSpot Dashboard Data
│  ├─ Total Leads (Today)
│  ├─ Stage Breakdown
│  ├─ Pipeline Value
│  ├─ Meetings Scheduled
│  └─ Deals Closed (Last 7 days)
│
├─ GET: Gmail Stats
│  ├─ Emails Sent (Yesterday)
│  ├─ Open Rate
│  ├─ Reply Rate
│  └─ Bounces
│
├─ GET: Asana Tasks
│  ├─ Overdue Tasks
│  ├─ Due Today
│  ├─ Due This Week
│  └─ Completed Yesterday
│
├─ FORMAT: Email Digest
│  Subject: "📊 Sales Pipeline Update - May 25, 2024"
│  
│  Body:
│  ─────────────────────────────────────────
│  PIPELINE STATUS
│  Leads: 24 | Qualified: 18 | Negotiation: 5 | Closed: 3
│  Pipeline Value: $840,000
│  
│  YESTERDAY'S ACTIVITY
│  New Leads: 3
│  Emails Sent: 12
│  Meetings: 2
│  Deals Closed: 1
│  
│  ACTION ITEMS
│  Overdue Follow-ups: 3
│  Due Today: 7
│  This Week: 21
│  
│  TOP OPPORTUNITIES
│  1. {{company1}} - {{dealValue1}} - {{stage1}}
│  2. {{company2}} - {{dealValue2}} - {{stage2}}
│  3. {{company3}} - {{dealValue3}} - {{stage3}}
│  
│  Have a great day! 🚀
│  ─────────────────────────────────────────
│
└─ GMAIL: Send to Sales Team
```

---

## 🔐 Webhook Security

### How to Secure Your Webhook

**Option 1: Zapier Webhook URL (Recommended)**
```
Zapier provides a unique URL for each zap:
https://hooks.zapier.com/hooks/catch/YOUR_ID/YOUR_SECRET/

This URL is:
✓ Unique to your zap
✓ Hard to guess
✓ Rate-limited
✓ Monitored for suspicious activity
```

**Option 2: Add Authentication Token**
```
Webhook URL:
https://your-domain.com/webhook/sales?token=YOUR_SECRET_TOKEN

Validation in JavaScript:
const token = req.query.token;
if (token !== process.env.WEBHOOK_TOKEN) {
  return res.status(401).json({ error: "Unauthorized" });
}
```

**Option 3: IP Whitelist (If using custom server)**
```
Allow only these Zapier IP ranges:
- 192.0.2.1/24
- 198.51.100.1/24
- 203.0.113.1/24

Check latest at: https://zapier.com/help/doc/ip-addresses
```

---

## 📊 Performance Optimization

### Throttling Rules
```
Per Zap (Free Tier):
- Max 100 tasks/month
- 1 task per execution

Per Zap (Premium):
- Max 30,000 tasks/month
- Multiple actions per execution
- Parallel processing enabled
```

### Best Practices
1. **Use Delays**: Don't send 50 emails instantly
2. **Batch Operations**: Group similar actions together
3. **Filter Data**: Only process relevant contacts
4. **Archive Old Data**: Keep active leads only
5. **Test Mode**: Always test before activating

---

## 🧪 Testing Your Workflows

### Manual Test in Zapier
```
1. Go to your Zap
2. Click "Test Trigger"
3. Select a real or sample contact
4. Click "Send Test"
5. View webhook payload that's generated
6. Check if actions execute correctly
```

### Test in Browser Console
```javascript
// Simulate webhook call
const testPayload = {
  action: "new_lead",
  lead: {
    name: "Test Lead",
    email: "test@example.com",
    company: "Test Co"
  }
};

fetch('https://hooks.zapier.com/hooks/catch/YOUR_ID/YOUR_SECRET/', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(testPayload)
})
.then(r => r.json())
.then(d => console.log('Success:', d));
```

### Check Webhook Logs
In your app's Settings tab:
```
1. Go to Settings
2. Click "Zapier Webhook Configuration"
3. View recent payloads and responses
4. Check for errors or failed deliveries
```

---

## 🚨 Common Issues & Solutions

### Issue: "Zap Paused"
```
Reason: Too many consecutive errors
Solution:
1. Check zap history for error messages
2. Fix the issue (bad email, missing field, etc.)
3. Click "Resume" to restart zap
```

### Issue: "Action Failed - Missing Field"
```
Reason: HubSpot field doesn't match Zapier field name
Solution:
1. Go to HubSpot → Settings → Properties → Contacts
2. Verify field name matches exactly
3. Use field ID instead of name if needed
4. Re-test the action
```

### Issue: "Email Already Exists"
```
Reason: Duplicate contact in HubSpot
Solution:
1. Add filter: "Skip if Email Already Exists"
2. Or update existing contact instead of creating
3. Use HubSpot's duplicate management settings
```

### Issue: "Slack Message Not Posting"
```
Reason: Bot not in channel or permissions denied
Solution:
1. Go to Slack workspace settings
2. Find "Zapier" bot
3. Check it's in #sales and #sales-alerts channels
4. Verify "Post messages" permission is enabled
5. Re-authenticate Zapier in Slack
```

---

## 📈 Monitoring & Analytics

### Track These Metrics
```
Zap Execution:
- Success rate (should be 95%+)
- Average execution time
- Peak usage hours
- Error types and frequency

Lead Quality:
- Leads per day
- Stage progression rate
- Time in each stage
- Conversion rate by source

Email Performance:
- Send rate
- Open rate
- Click rate
- Reply rate
- Bounce rate

Sales Team:
- Task completion rate
- Average response time
- Deal closing rate
- Revenue impact
```

### Create a Dashboard
In Google Sheets:
```
=IMPORTDATA("https://hooks.zapier.com/export/YOUR_ZAP_ID")
```

---

## 🔄 Workflow Maintenance

### Monthly Checklist
- [ ] Review zap success rates
- [ ] Check for failed tasks
- [ ] Update email templates
- [ ] Review lead scoring rules
- [ ] Check Slack channel activity
- [ ] Verify all integrations still connected
- [ ] Clean up old/inactive leads
- [ ] Test each workflow manually

### Quarterly Review
- [ ] Analyze pipeline metrics
- [ ] Identify bottlenecks
- [ ] Optimize workflow order
- [ ] Add new workflows as needed
- [ ] Train team on updates
- [ ] Review integration costs
- [ ] Plan next quarter improvements

---

## 🎓 Advanced Tips

### Use Formatter for Data Cleanup
```
Zap Step: Zapier Formatter → Text

Input: "  john.smith@company.com  "
Functions:
- Trim whitespace
- Convert to lowercase
- Remove special characters

Output: "john.smith@company.com"
```

### Create Conditional Logic
```
Filter Step:
IF Contact Score > 50 AND Company Size > 100
THEN Send high-priority alert
ELSE Add to nurture sequence
```

### Use Lookups for Context
```
When creating Asana task:
1. Use HubSpot Lookup to get {{salesRepEmail}}
2. Use Gmail Lookup to get {{lastEmailSent}}
3. Use Slack Lookup to get {{salesChannelId}}
4. Combine data before taking action
```

---

**Happy automating!** 🎉

For more help, visit [zapier.com/help](https://zapier.com/help)
