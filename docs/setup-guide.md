# Setup Guide

This guide walks you through setting up the AI Health WhatsApp Bot from scratch.

## Prerequisites

- Meta Business Account with WhatsApp Business API access
- n8n instance (cloud or self-hosted)
- OpenAI API account
- ElevenLabs account (for voice features)

---

## Step 1: WhatsApp Business API Setup

### 1.1 Create Meta Business Account

1. Go to [Meta Business Suite](https://business.facebook.com/)
2. Create a Business Account if you don't have one
3. Navigate to **WhatsApp** section

### 1.2 Set Up WhatsApp Business API

1. Go to [Meta for Developers](https://developers.facebook.com/)
2. Create a new App → Select "Business" type
3. Add WhatsApp product to your app
4. Note down:
   - Phone Number ID
   - WhatsApp Business Account ID
   - Access Token (generate a permanent one)

### 1.3 Configure Webhook

1. In your app settings, go to WhatsApp → Configuration
2. Set Webhook URL: `https://your-n8n-instance.com/webhook/whatsapp-webhook`
3. Set Verify Token: (create a random string, save it)
4. Subscribe to: `messages`

---

## Step 2: n8n Setup

### 2.1 Install n8n

**Option A: Cloud (Easiest)**
- Sign up at [n8n.cloud](https://n8n.cloud)

**Option B: Self-hosted**
```bash
# Using Docker
docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### 2.2 Import Workflow

1. Open n8n dashboard
2. Go to Workflows → Import from File
3. Select `n8n/workflow.json` from this repository
4. The workflow will be imported with placeholder credentials

### 2.3 Configure Credentials

Create the following credentials in n8n:

**OpenAI:**
- Type: OpenAI API
- API Key: Your OpenAI API key

**Update nodes with your values:**
- WhatsApp Webhook → Your webhook path
- OpenAI Chat → Select your OpenAI credentials
- ElevenLabs TTS → Add your API key and voice ID
- Send WhatsApp Reply → Add your Phone Number ID and Access Token

---

## Step 3: OpenAI Setup

1. Go to [OpenAI Platform](https://platform.openai.com/)
2. Create an account and add billing
3. Generate an API key
4. Add to n8n credentials

---

## Step 4: ElevenLabs Setup (Optional - for voice)

1. Sign up at [ElevenLabs](https://elevenlabs.io/)
2. Get your API key from Profile → API Key
3. Choose a voice and note the Voice ID
4. Add to n8n workflow

---

## Step 5: Testing

### Local Testing

1. Activate the workflow in n8n
2. Send a test message to your WhatsApp Business number
3. Check n8n execution logs for any errors

### Webhook Verification

Meta will send a verification request to your webhook. The n8n workflow handles this automatically.

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Webhook not receiving messages | Check URL is HTTPS and publicly accessible |
| OpenAI errors | Verify API key and billing is set up |
| WhatsApp reply fails | Check Phone Number ID and Access Token |
| Voice not working | Verify ElevenLabs API key and voice ID |

### Debug Mode

Enable debug mode in n8n to see full request/response data for troubleshooting.

---

## Security Checklist

- [ ] All API keys stored as credentials, not in workflow
- [ ] Webhook URL is HTTPS
- [ ] Verify token is random and secure
- [ ] No user data is logged or stored
- [ ] Rate limiting configured

---

## Next Steps

- Customize prompts in `prompts/health-assistant.md`
- Add more language support
- Implement image analysis with GPT-4 Vision
- Add location-based hospital search
