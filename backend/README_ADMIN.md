# 🔐 ExamForge Admin Guide

Complete guide for managing ExamForge orders, generating codes, and processing payments.

---

## 🚀 Initial Setup (One-Time)

### **Step 1: Configure PayPal Return URL**

This is **CRITICAL** for payment verification.

1. Go to **PayPal dashboard** (where you created payment links)
2. Click on your payment link (e.g., `QSCW64U45CG22`)
3. Go to **"Confirmation"** tab
4. Enable **"Auto-return URL"** ✅
5. Enter your app URL + `/payment-success`:

```
https://your-netlify-app.netlify.app/payment-success
```

**Example:**
```
https://melodious-biscotti-d8ef3c.netlify.app/payment-success
```

6. Click **"Update"** to save
7. ✅ Repeat for all 3 payment links (Starter, Popular, Power)

### **Step 2: Set Your Admin Secret**

In **Railway Variables**:
```
ADMIN_SECRET = your-secret-password (choose something strong!)
```

### **Step 3: Access Admin Dashboard**

```
https://your-railway-url/admin?secret=your-secret-password
```

---

## 📊 Admin Dashboard Overview

### **Dashboard Stats (Top Cards)**

| Card | Shows |
|------|-------|
| ⏳ **Pending** | Orders waiting for code generation |
| ✅ **Sent** | Completed orders (code sent) |
| 💰 **Revenue** | Total money from completed orders |
| 👥 **Devices** | Number of unique users |
| ⚡ **Credits** | Total credits issued |

---

## 📋 Order Management Workflow

### **Complete Order Lifecycle**

```
1. Customer enters WhatsApp + clicks PayPal
   ↓
2. PayPal payment window opens
   ↓
3a. Customer closes without paying
    → NO order created ✓
    
3b. Customer completes payment
    → PayPal redirects to /payment-success
    → Order AUTOMATICALLY created ✓
    ↓
4. Admin dashboard shows "PENDING" order
   ↓
5. Admin verifies payment in PayPal
   ↓
6. Admin clicks "🔍 Verify Payment" button
   ↓
7. Admin clicks "⚡ Gen Code" → Code generated
   ↓
8. Admin clicks "💬 Send" → WhatsApp opens
   ↓
9. Admin pastes code in WhatsApp + sends to customer
   ↓
10. Customer receives code → Pastes in app
    ↓
11. App validates → Credits added instantly
    ↓
12. Admin clicks "✅ Mark Sent" → Order complete
```

---

## 🎯 Step-by-Step Management

### **For Each Pending Order:**

#### **Step 1: Verify Payment**

Before generating any code, verify the customer actually paid:

1. Find the **pending order** in admin dashboard
2. Check the **WhatsApp number**
3. Go to **PayPal** and search for payment from that number
4. Confirm payment is **completed** ✓
5. In admin panel, click **"🔍 Verify Payment"** button

**What happens:**
- Order moves from "unverified" to "verified"
- "🔍 Verify" button changes to "⚡ Gen Code"

#### **Step 2: Generate Code**

Once payment is verified:

1. Click **"⚡ Gen Code"** button
2. New code appears (e.g., `EFP-A1B2-C3D4`)
3. Copy button shows: **"📋 Copy"**

**Code Format:**
- **EFS** = Starter pack (100 credits)
- **EFP** = Popular pack (300 credits)
- **EFW** = Power pack (600 credits)

#### **Step 3: Send Code via WhatsApp**

1. Click **"💬 Send"** button
2. WhatsApp window opens with pre-filled message
3. Customer's WhatsApp number is already filled
4. Code is in the message: `"Your access code is: EFP-A1B2-C3D4"`
5. Click **"Send"** in WhatsApp
6. Message sent! ✅

#### **Step 4: Mark as Sent**

After WhatsApp message is sent:

1. Return to admin panel
2. Click **"✅ Mark Sent"** button
3. Order status changes to **"SENT"** ✅
4. Page refreshes
5. Order moves to completed section

---

## 🗑️ Delete Unsuccessful Orders

If a customer doesn't pay:

1. Check **PayPal** — no payment received
2. In admin panel, click **"🗑 Delete"** button
3. Confirm deletion popup
4. Order is **removed** from dashboard ✅
5. No fake pending orders cluttering your dashboard

**When to delete:**
- Customer clicked PayPal but closed without paying
- Accidental order creation
- Test orders during setup

---

## 📈 Daily Workflow Example

### **Morning Check (10 minutes)**

```
1. Open admin dashboard
2. Check "Pending Orders" count
3. For each pending order:
   a) Verify payment in PayPal (1 min)
   b) Click "🔍 Verify Payment" (5 seconds)
   c) Click "⚡ Gen Code" (1 second)
   d) Click "💬 Send" → Send on WhatsApp (30 seconds)
   e) Return to app, click "✅ Mark Sent" (5 seconds)
   
4. Check stats
   - Revenue updated?
   - Credits issued increased?
   - Devices growing?
```

**Time per order: ~2 minutes**

---

## 🔧 Configuration & Settings

### **Backend Environment Variables**

Set these in **Railway Dashboard → Variables**:

```yaml
ADMIN_SECRET: your-secret-admin-password
AI_KEY: sk-or-YOUR-OPENROUTER-API-KEY
AI_MODEL: anthropic/claude-haiku-4
PORT: 3001
DB_PATH: /app/data
```

### **Changing AI Model**

Want to switch models without redeploying?

1. Go to **Railway → Variables**
2. Change `AI_MODEL` to a new value:

```
anthropic/claude-3-haiku        (fast + cheap)
anthropic/claude-3-5-haiku      (very fast + cheaper)
anthropic/claude-haiku-4        (current)
meta-llama/llama-3.3-70b:free   (FREE option)
```

3. **No redeployment needed** — takes effect immediately ✅

### **Common Model Choices**

| Model | Speed | Cost | Quality | Best For |
|-------|-------|------|---------|----------|
| claude-haiku-4 | ⚡⚡⚡ | $ | Good | Default choice |
| claude-3-haiku | ⚡⚡⚡⚡ | $$ | Good | Faster generation |
| llama-70b:free | ⚡⚡ | FREE | Decent | Cost-cutting |

---

## 📊 Reports & Analytics

### **Viewing Stats**

Admin dashboard shows:
- **Pending Orders** — awaiting payment verification
- **Orders Sent** — completed (code sent to customer)
- **Total Revenue** — sum of completed order prices
- **Active Devices** — unique customers
- **Total Credits Issued** — credits given out

### **Export Orders (Manual)**

Admin dashboard shows order table with:
- WhatsApp number
- Pack size
- Price paid
- Status (pending/sent)
- Date created

To export:
- Screenshot or copy-paste table
- Use browser's "Save as PDF" feature

---

## 🆘 Troubleshooting

### **Issue: Order not appearing in dashboard**

**Solution:**
1. Refresh page (Ctrl+R or Cmd+R)
2. Check if Payment was actually completed in PayPal
3. If not completed, customer needs to retry payment
4. If completed, wait 30 seconds and refresh

### **Issue: Customer says code didn't arrive**

**Solution:**
1. Check admin panel — did you click "💬 Send"?
2. Ask customer to check WhatsApp spam folder
3. You can resend by clicking "💬 Send" again
4. If still no SMS, verify correct WhatsApp number

### **Issue: Payment verification button missing**

**Solution:**
1. This order is already verified
2. Click "⚡ Gen Code" to generate code
3. If no code button, order already has a code

### **Issue: Admin dashboard showing 404 error**

**Solution:**
1. Check URL: `https://YOUR-RAILWAY-URL/admin?secret=YOUR-SECRET`
2. Verify `ADMIN_SECRET` in Railway variables
3. Refresh browser cache (Ctrl+Shift+R)
4. If still error, check Railway deployment status

### **Issue: Customer can't redeem code in app**

**Solution:**
1. Verify code format is correct (e.g., `EFS-A1B2-C3D4`)
2. Code should be UPPERCASE
3. No spaces in code
4. Code must be for correct payment link (Starter/Popular/Power)
5. Test code in app yourself to verify

---

## 💡 Best Practices

✅ **Verify before generating** — Always check PayPal first  
✅ **Generate immediately** — Don't delay, customer is waiting  
✅ **Send same day** — Customer expects code within 5 minutes  
✅ **Mark sent** — Update status so you know what's done  
✅ **Delete failed** — Clean dashboard of unpaid orders  
✅ **Check revenue daily** — Track business metrics  
✅ **Backup regularly** — Your data is on Railway's servers  

---

## 📞 Support

### **For Admin/Technical Issues:**
- Check Railway **Console** logs for errors
- Verify environment variables are set correctly
- Check that PayPal return URL is configured
- Ensure admin secret matches in admin panel login

### **For Customer Issues:**
- Help with code redemption
- Resend codes if they don't arrive
- Verify WhatsApp number is correct
- Troubleshoot payment issues

### **Security:**
- Keep `ADMIN_SECRET` private — never share
- Don't post admin URLs in public places
- Log out of admin when done
- Use strong admin password

---

## 📋 Checklist for Launch

- [ ] Configure PayPal auto-return URL for all 3 payment links
- [ ] Set `ADMIN_SECRET` in Railway
- [ ] Test payment flow (buy 1 credit as test)
- [ ] Access admin dashboard successfully
- [ ] Generate test code
- [ ] Send test code via WhatsApp
- [ ] Verify code can be redeemed in app
- [ ] Check stats update correctly
- [ ] Test deleting an order
- [ ] Share admin URL securely with team

---

**You're all set! Happy managing! 🚀**

*For the latest updates, check the main README.txt in backend folder*
