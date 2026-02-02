# Meal Prep Pro 🍽️

A personal meal prep and calorie tracking app with Pakistani dishes, AI-powered recipe generation, and smart grocery lists.

## Features

- 📅 2-week rotating meal plan
- 🍳 Breakfast tracking with AI suggestions
- 🛒 Smart grocery lists organized by store (Walmart/Apna)
- 🤖 AI-powered recipe generation (paste from web or type name)
- 📊 Macro tracking (protein, calories, carbs, fat)
- ☁️ **Cloud sync across all devices** (via Supabase)
- 💾 JSON export/import for backups
- 📱 Mobile-friendly PWA design

---

## 🚀 Deployment (5 minutes)

### Step 1: Deploy to Vercel (Free)

```bash
# In the meal-prep-pro-deploy folder
git init
git add .
git commit -m "Initial commit"

# Create private GitHub repo and push
gh repo create meal-prep-pro --private --source=. --push
```

Then:
1. Go to [vercel.com](https://vercel.com)
2. Sign in with GitHub
3. Click "Add New Project" → Import `meal-prep-pro`
4. Click Deploy

✅ Your app is now live at `https://meal-prep-pro-xxx.vercel.app`

---

## ☁️ Cloud Sync Setup (5 minutes)

To sync data across all your devices (phone, laptop, tablet):

### Step 1: Create Supabase Account

1. Go to [supabase.com](https://supabase.com)
2. Sign up (free tier: 500MB storage, unlimited API calls)
3. Click "New Project"
4. Name it anything (e.g., "meal-prep")
5. Set a database password (save it somewhere)
6. Choose a region close to you
7. Click "Create new project"

### Step 2: Create Database Table

1. In Supabase dashboard, click **SQL Editor** (left sidebar)
2. Click "New query"
3. Paste this SQL and click "Run":

```sql
-- Create the sync table
CREATE TABLE IF NOT EXISTS meal_prep_data (
  sync_code TEXT PRIMARY KEY,
  data JSONB NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Enable row level security
ALTER TABLE meal_prep_data ENABLE ROW LEVEL SECURITY;

-- Allow all operations (your sync code keeps it private)
CREATE POLICY "Allow all" ON meal_prep_data
  FOR ALL USING (true) WITH CHECK (true);

-- Enable realtime sync
ALTER publication supabase_realtime ADD TABLE meal_prep_data;
```

### Step 3: Get Your API Keys

1. Go to **Settings** → **API** (in left sidebar)
2. Copy these values:
   - **Project URL**: `https://xxxxx.supabase.co`
   - **anon public key**: `eyJhbGciOiJIUzI1NiIs...`

### Step 4: Configure Your App

1. Open your Meal Prep Pro app
2. Click ⚙️ Settings
3. Go to **☁️ Cloud Sync** tab
4. Paste your:
   - Supabase Project URL
   - Supabase Anon Key
5. Click "Generate" to create your sync code (e.g., `MPP-ABC123`)
6. Click "Save & Connect"

### Step 5: Sync Other Devices

On each device:
1. Open your Meal Prep Pro app
2. Go to Settings → Cloud Sync
3. Enter the same Supabase URL and Key
4. Enter your **same sync code** (e.g., `MPP-ABC123`)
5. Click "Save & Connect"
6. Click "↓ Pull" to download your data

🎉 **Done!** Changes now sync automatically across all devices.

---

## 🔒 Privacy & Security

- ✅ **PIN Lock** - 4-6 digit PIN required to access app
- ✅ **Your data stays yours** - Only you have your sync code
- ✅ **API keys stored locally** - Never sent to any server except Supabase
- ✅ **Private by default** - Vercel URL is unguessable
- ✅ **No tracking** - Zero analytics or cookies
- ✅ **Open source** - Inspect every line of code

---

## 💾 Backup Options

### Option 1: Supabase (Real-time Sync)
Best for: Auto-sync across devices
- Changes sync automatically
- Real-time updates
- See [Cloud Sync Setup](#️-cloud-sync-setup-5-minutes) above

### Option 2: JSONBin.io (Simple Backup)
Best for: Manual backups, simpler setup

1. Go to [jsonbin.io](https://jsonbin.io) and sign up (free)
2. Copy your **X-Master-Key** from dashboard
3. In app: Settings → Backup → Paste key
4. Click "Push to Cloud" to backup
5. Save your **Bin ID** to restore on other devices

Free tier: 10,000 requests/month

### Option 3: Local JSON Export
Best for: Offline backups, file transfers

1. Settings → Backup → Export JSON
2. Save file to your device
3. Import on other devices

---

## 📱 Mobile Install (PWA)

**iPhone/iPad:**
1. Open your app URL in Safari
2. Tap Share → "Add to Home Screen"

**Android:**
1. Open your app URL in Chrome
2. Tap ⋮ menu → "Add to Home screen"

---

## 🔧 Tech Stack

- React 18 (via CDN - no build step)
- Tailwind CSS
- Supabase (PostgreSQL + Realtime)
- Claude AI API
- localStorage + cloud sync

---

## 📋 Alternative Deployment Options

### Netlify (Free)
```bash
# Same git setup, then:
# Go to netlify.com → Add new site → Import from Git
```

### GitHub Pages (Free)
```bash
# In repo settings → Pages → Deploy from branch
# Select: main branch, /public folder
```

### Run Locally
```bash
npx serve public
# Opens at http://localhost:3000
```

---

## 💡 Tips

- **Backup regularly**: Use Settings → Backup → Export JSON
- **Sync code is secret**: Treat it like a password
- **Pull before edit**: On new devices, pull cloud data first
- **Push after changes**: Click the cloud icon to sync changes

---

## 🤖 Claude API

Get your API key from: https://console.anthropic.com/settings/keys

Used for:
- AI recipe generation from names
- Parsing pasted web recipes
- Smart breakfast suggestions
- Grocery list organization
