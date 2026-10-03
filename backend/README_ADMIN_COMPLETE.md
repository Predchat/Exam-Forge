# 🔐 ExamForge Admin - Complete Management Guide

**For:** Business owners, system administrators  
**Purpose:** Manage orders, generate codes, monitor revenue  
**Time to learn:** 15 minutes  

---

## 📋 Quick Reference

### **Admin Dashboard URL**
```
https://your-railway-url/admin?secret=your-admin-password
```

### **Key Numbers to Remember**
- **Starter:** EFS = $5 for 100 credits
- **Popular:** EFP = $12 for 300 credits  
- **Power:** EFW = $20 for 600 credits

### **Top 3 Things You'll Do**
1. 🔍 **Verify Payment** - Check PayPal, confirm payment received
2. ⚡ **Generate Code** - Create access code for customer
3. 💬 **Send Code** - WhatsApp message to customer

---

## 🚀 Complete Setup (30 Minutes)

### **Phase 1: Environment Setup (5 min)**

**Set these in Railway Variables:**

```
ADMIN_SECRET = your-unique-admin-password
AI_KEY = sk-or-your-openrouter-api-key
AI_MODEL = anthropic/claude-haiku-4
PORT = 3001
```

**Why each variable:**
- `ADMIN_SECRET` → Login password for admin dashboard
- `AI_KEY` → Powers exam generation (get from openrouter.io)
- `AI_MODEL` → Which AI to use (Claude, Llama, etc.)
- `PORT` → Server port (keep as 3001)

### **Phase 2: PayPal Integration (15 min)**

**For EACH payment link (3 total):**

1. **Create Payment Link in PayPal**
   - Go to PayPal dashboard
   - Create new payment link
   - Set amount: $5 (Starter), $12 (Popular), $20 (Power)
   - Get link ID: `https://www.paypal.com/ncp/payment/ABC123`

2. **Enable Auto-Return URL**
   - Open payment link settings
   - Click "Confirmation" tab
   - Toggle "Auto-return URL" ON ✅
   - Enter: `https://your-netlify-app.netlify.app/payment-success`
   - Click "Update"

3. **Update in App**
   - Open `app/index.html`
   - Find `const PAYPAL = { ... }`
   - Replace with your 3 links:
   ```javascript
   const PAYPAL = {
     starter: 'https://www.paypal.com/ncp/payment/YOUR_ID_1',
     popular: 'https://www.paypal.com/ncp/payment/YOUR_ID_2',
     power:   'https://www.paypal.com/ncp/payment/YOUR_ID_3',
   };
   ```
   - Save and redeploy to Netlify

4. **Test Payment**
   - Open your app
   - Try full payment flow (test account)
   - Verify redirects to /payment-success ✅
   - Check admin sees new order

### **Phase 3: Admin Access (5 min)**

1. **Access Dashboard**
   ```
   https://your-railway-url/admin?secret=your-password
   ```

2. **Bookmark for Quick Access**
   - Save as browser bookmark
   - Or add to home screen

3. **Test Functionality**
   - View orders
   - Try verify button
   - Try generate code
   - Try delete button

---

## 📊 Admin Dashboard Explained

### **Top Statistics Cards**

```
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│ ⏳ PENDING│ ✅ SENT  │ 💰 $XXXX │ 👥 200   │ ⚡ 5000K │
│    3     │    12    │ REVENUE  │ DEVICES  │ CREDITS  │
└──────────┴──────────┴──────────┴──────────┴──────────┘
```

**What each shows:**
- **⏳ Pending** = Orders awaiting action (verify → generate → send)
- **✅ Sent** = Completed orders (customer received code)
- **💰 Revenue** = Total money from completed orders
- **👥 Devices** = Number of unique customers
- **⚡ Credits** = Total credits issued to customers

### **Order List View**

Each order shows:
```
╔════════════════════════════════════════════════════╗
║ WhatsApp: +94771234567                             ║
║ Pack: Starter | Price: $5 | Status: PENDING      ║
║ Created: 6/6/2024                                  ║
╠════════════════════════════════════════════════════╣
║ [🔍 Verify Payment] [🗑 Delete]                    ║
║ ⚠️ Check PayPal or delete if unpaid               ║
╚════════════════════════════════════════════════════╝
```

---

## ✅ Daily Management Workflow

### **Morning (15 min)**

```
1. Open admin dashboard (⏰ 5 min)
   - View stats
   - Check pending count
   - Skim recent orders

2. Process pending orders (⏰ 10 min per order)
   - Verify payment in PayPal
   - Click "🔍 Verify Payment"
   - Click "⚡ Gen Code"
   - Click "💬 Send" → WhatsApp
   - Send code message
   - Return to app, click "✅ Mark Sent"

3. Total time: 2-3 minutes per order
```

### **Afternoon**

- Check for new orders
- Process any new payments
- Respond to customer support

### **End of Day**

- Check total revenue
- Note number of sent orders
- Monitor payment success rate

---

## 🎯 Complete Order Processing Guide

### **Order Status Flow**

```
PENDING (unverified)
    ↓
[Click: 🔍 Verify Payment]
    ↓
PENDING (verified) — CODE NOT GENERATED
    ↓
[Click: ⚡ Gen Code]
    ↓
PENDING (verified) — CODE GENERATED
    ↓
[Click: 💬 Send]
[Send via WhatsApp]
    ↓
[Click: ✅ Mark Sent]
    ↓
SENT — COMPLETE ✅
```

### **Step 1: Verify Payment**

**What to do:**
1. See pending order with WhatsApp number
2. Go to **PayPal account**
3. Search for payment from that customer
4. Confirm payment is **"Completed"** (not pending/failed)

**In admin panel:**
- Click **"🔍 Verify Payment"** button
- Order status updates internally
- Button changes to **"⚡ Gen Code"**

**Why this step?**
- Prevents generating codes for failed payments
- Ensures customer actually paid
- Protects your revenue

### **Step 2: Generate Code**

**What happens:**
- System generates unique code
- Code appears in order
- Format: `EFS-XXXX-XXXX` (Starter), `EFP-...` (Popular), `EFW-...` (Power)

**In admin panel:**
1. Click **"⚡ Gen Code"** button
2. Code generates instantly
3. Code appears with buttons:
   - **📋 Copy** - Copy code to clipboard
   - **💬 Send** - Opens WhatsApp

**Code Details:**
- Each code = one-time use
- Valid immediately
- No expiration date
- Can't reuse same code

### **Step 3: Send via WhatsApp**

**What happens:**
1. WhatsApp window opens
2. Customer's WhatsApp number pre-filled
3. Message template pre-filled with code

**In WhatsApp:**
```
Your access code is: EFS-A1B2-C3D4
```

**Options:**
- Click "Send" to send
- Edit message if needed
- Add personal greeting
- Reorder for better messaging

**Confirmation:**
- Message shows "Delivered" ✅
- Don't mark as sent until WhatsApp confirms
- Customer should receive within 1 min

### **Step 4: Mark as Sent**

**When to click:**
- After customer confirms they received code
- OR 2 minutes after WhatsApp shows delivered
- OR after you physically sent it

**In admin panel:**
1. Click **"✅ Mark Sent"** button
2. Order status changes to **"SENT"**
3. Order moves to "Completed" section
4. Revenue updates automatically

**Why important:**
- Completes the transaction
- Updates revenue stats
- Shows order was successfully fulfilled

---

## 🗑️ Deleting Unsuccessful Orders

### **When to Delete**

Delete orders if:
- ❌ Customer clicked PayPal but didn't pay
- ❌ Payment failed
- ❌ Accidental/test order
- ❌ Customer asked to cancel
- ❌ More than 24 hours old with no payment

### **How to Delete**

1. Find pending order (unverified)
2. Click **"🗑 Delete"** button
3. Confirm in popup: "Delete order from [WhatsApp]?"
4. Click "Yes, delete"
5. Order removed from dashboard ✅

**Important:**
- Can't delete verified/sent orders
- Deleted orders can't be recovered
- Double-check before deleting

### **Benefits**

- ✅ Cleaner dashboard
- ✅ Easier to track real orders
- ✅ Better metrics (no fake pending)
- ✅ Less confusion

---

## 📊 Analytics & Reporting

### **Key Metrics**

**Track daily:**
- Pending orders (today)
- Sent orders (today)
- Revenue (today/month)
- Average time per order

**Calculate:**
- **Conversion Rate** = Sent / Total Orders
- **Average Order Value** = Total Revenue / Sent Orders
- **Revenue/Day** = Total Revenue / Days Since Launch

**Target benchmarks:**
- ⏳ Process pending orders: < 30 min
- ✅ Send codes: within 5 min
- 💰 Revenue/month: depends on traffic
- 👥 Customer base: grows over time

### **Reporting (Optional)**

**Weekly Report:**
```
Week of June 6-12:
- Total orders: 15
- Completed: 12 (80% rate)
- Revenue: $140
- Avg per order: $11.67
- Devices acquired: 12
```

---

## 🔧 Configuration & Customization

### **Change Pricing**

To change pack prices:

1. **Update PayPal**
   - Create new payment links with new prices
   - Copy new link IDs

2. **Update App**
   - Edit `app/index.html`
   - Update PAYPAL object with new links

3. **Update Backend** (if needed)
   - PACKS object shows server-side amounts
   - Match what you put in PayPal

### **Change AI Model**

To use different AI:

1. **Go to Railway Variables**
2. Change `AI_MODEL` to:
   ```
   anthropic/claude-haiku-4         (default)
   anthropic/claude-3-5-haiku       (faster)
   meta-llama/llama-3.3-70b:free   (free tier)
   ```
3. **No redeployment needed** — changes immediately

### **Add New Subjects**

Subjects are customizable by students (no admin needed).  
But if you want to pre-populate:

1. Edit `app/index.html`
2. Find `const SUBJECTS = [ ... ]`
3. Add new subjects to array
4. Redeploy to Netlify

---

## 🆘 Common Issues & Fixes

### **Order Not Appearing**

```
Cause: Payment not yet synced
Fix:
1. Wait 30 seconds
2. Refresh dashboard
3. Check PayPal - payment completed?
4. If not completed, order won't create
```

### **Code Not Generating**

```
Cause: Not verified yet
Fix:
1. Click "🔍 Verify Payment" first
2. Wait 5 seconds
3. Click "⚡ Gen Code"
```

### **WhatsApp Won't Open**

```
Cause: Browser blocking popup
Fix:
1. Copy code manually (📋 Copy button)
2. Open WhatsApp manually
3. Send message with code
4. Or click "💬 Send" again
```

### **Can't Delete Order**

```
Cause: Order already sent/verified
Fix:
1. Can only delete unverified pending
2. Delete button only shows for those
3. If verified, you must contact support
```

### **Admin Dashboard 404**

```
Cause: Wrong URL or secret
Fix:
1. Check URL format
2. Verify secret matches ADMIN_SECRET
3. Try different browser
4. Clear cache (Ctrl+Shift+R)
```

### **Revenue Not Calculating**

```
Cause: Orders not marked as "Sent"
Fix:
1. Click "✅ Mark Sent" for completed orders
2. Revenue updates only for Sent orders
3. Refresh page to see update
```

---

## 🔐 Security & Best Practices

### **Admin Dashboard Security**

✅ **DO:**
- Keep admin URL private
- Use strong ADMIN_SECRET (20+ characters)
- Change password monthly
- Log out when done
- Use HTTPS only (both Netlify & Railway use it)

❌ **DON'T:**
- Share admin URL publicly
- Use simple passwords (12345, password)
- Give access to untrustworthy people
- Leave admin logged in overnight
- Write password in notes/files

### **Payment Security**

✅ **Verify before generating**
- Always check PayPal
- Confirm payment is completed
- Don't trust pending payments
- Screenshot proof if needed

✅ **Monitor for fraud**
- Watch for repeated small payments
- Check for unusual WhatsApp numbers
- Track revenue patterns
- Report suspicious activity

### **Data Protection**

✅ **Customer WhatsApp numbers**
- Keep private
- Don't share publicly
- Don't use for marketing (unless opted in)

✅ **Order data**
- Stored on Railway securely
- HTTPS encryption in transit
- Backup regularly (optional)

---

## 📈 Growth Tips

### **Increase Sales**

1. **Share with Students**
   - Tell friends about free exams
   - Marketing on student groups
   - Offer referral discounts

2. **Build Trust**
   - Fast code delivery (< 5 min)
   - Professional communication
   - Good customer service
   - Reliable platform

3. **Add More Subjects**
   - Let students customize subject names
   - Support multiple languages
   - Add exam history tracking

4. **Collect Feedback**
   - Ask what features students want
   - Plan v3.0 features
   - Improve based on usage

### **Scale Operations**

1. **Hire Help**
   - Train assistant to verify payments
   - Share admin access (use same secret)
   - Split processing tasks

2. **Automate**
   - Set up auto-responses
   - Batch process orders
   - Use scheduling tools

3. **Track KPIs**
   - Daily revenue
   - Customer acquisition cost
   - Lifetime value per customer
   - Conversion rate

---

## 📞 Support Resources

### **Troubleshooting Checklist**

- [ ] Check Railway deployment status
- [ ] Verify all environment variables set
- [ ] Check browser console for errors (F12)
- [ ] Try different browser
- [ ] Clear cache and reload
- [ ] Check network connectivity
- [ ] Review Railway logs

### **When to Escalate**

- Backend errors (500, 503)
- Database corruption
- Security issues
- PayPal integration problems
- Large-scale issues (50+ orders failing)

---

## ✨ You're Ready!

You now have all tools to:

✅ Manage orders professionally  
✅ Generate and send codes  
✅ Track revenue  
✅ Grow your business  
✅ Support customers  

**Start processing orders today!**

---

**Quick Links:**
- [User Guide](../app/README_USER.md)
- [Setup Guide](../README_SETUP.md)
- [App Manual](../app/README_SETUP_USER.md)

