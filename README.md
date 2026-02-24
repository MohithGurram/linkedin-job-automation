# 📊 LinkedIn Daily Job Report Automation
**Mohith Gurram — Data Engineer Job Search Automation**

Automatically scrapes LinkedIn for Data Engineer jobs posted in the last 24 hours in India, scores them against your resume, generates an Excel report, and emails it to **mohithgurram03@gmail.com** every day at **10:30 PM IST**.

---

## 🗂️ Project Structure

```
linkedin-job-automation/
├── .github/
│   └── workflows/
│       └── daily_job_report.yml   ← GitHub Actions scheduler
├── scripts/
│   └── generate_report.py         ← Main script (scrape → score → Excel → email)
├── requirements.txt
└── README.md
```

---

## ⚙️ One-Time Setup (takes ~10 minutes)

### Step 1 — Create a GitHub Repository

1. Go to [github.com](https://github.com) → **New repository**
2. Name it: `linkedin-job-automation`
3. Set it to **Private** (recommended)
4. Click **Create repository**
5. Upload all files from this folder into the repo (drag & drop or use Git)

---

### Step 2 — Get Your Apify API Token

1. Go to [apify.com](https://apify.com) and sign in (or create a free account)
2. Click your avatar → **Settings** → **Integrations**
3. Under **API tokens**, click **+ Create new token**
4. Name it `linkedin-jobs` → **Create**
5. Copy the token (starts with `apify_api_...`)

> 💡 **Free tier**: Apify gives $5/month free credits. This automation uses ~$0.10/day (100 results × $0.001), so you get **~50 days free** per month's credit. More than enough!

---

### Step 3 — Set Up Gmail App Password

Gmail requires an **App Password** (not your regular password) for SMTP sending.

1. Go to your Google Account → [myaccount.google.com](https://myaccount.google.com)
2. Navigate to **Security** → **2-Step Verification** (enable it if not already)
3. Go back to **Security** → scroll down → **App passwords**
4. Select app: **Mail** | Select device: **Other** → type `GitHub Actions`
5. Click **Generate** → Copy the 16-character password (e.g., `abcd efgh ijkl mnop`)

> ⚠️ Save this password — Google only shows it once!

---

### Step 4 — Add GitHub Secrets

In your GitHub repository:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret** for each of the following:

| Secret Name | Value | Example |
|-------------|-------|---------|
| `APIFY_API_TOKEN` | Your Apify API token | `apify_api_abc123...` |
| `EMAIL_SENDER` | Your Gmail address | `youremail@gmail.com` |
| `EMAIL_PASSWORD` | Gmail App Password (no spaces) | `abcdefghijklmnop` |

> 🔒 Secrets are encrypted — GitHub never exposes them in logs.

---

### Step 5 — Enable GitHub Actions

1. In your repo, click the **Actions** tab
2. If prompted, click **"I understand my workflows, go ahead and enable them"**
3. You're done! The workflow will now run automatically every day at **10:30 PM IST**

---

## 🧪 Test It Immediately (Manual Run)

Don't wait until 10:30 PM to verify it works:

1. Go to your repo → **Actions** tab
2. Click **"Daily LinkedIn Job Report"** in the left sidebar
3. Click **"Run workflow"** → **"Run workflow"** (green button)
4. Watch the run — it takes ~5–8 minutes
5. Check your email at mohithgurram03@gmail.com 📬

---

## 📧 What the Email Looks Like

**Subject:** `🔥 Daily LinkedIn Job Report — 28 Data Engineer Jobs | 2026-02-25`

**Body includes:**
- Total jobs found
- HIGH priority count
- Quick application tips

**Attachment:** `Mohith_LinkedIn_Jobs_2026-02-25.xlsx` with:
- **Sheet 1 — Job Listings**: All ranked jobs, filterable, with clickable Apply links
- **Sheet 2 — Dashboard**: Summary stats + HIGH priority shortlist

---

## 🎯 How Jobs Are Scored

| Component | Points |
|-----------|--------|
| Each resume keyword match (21 keywords) | +1 each (max 21) |
| Job title contains "data engineer" | +3 |
| Applicants < 100 | +2 |
| Applicants 100–150 | +1 |
| Posted today | +1 |
| **Maximum possible score** | **27** |

**Resume keywords tracked:** Azure, Databricks, Snowflake, PySpark, Spark, Kafka, Airflow, ADF, ADLS, Delta Lake, Python, SQL, ETL, ELT, Data Factory, Data Lake, Data Warehouse, Data Engineering, Hadoop, Hive, Teradata

**Priority levels:**
- 🔥 **HIGH** — Score ≥ 18, or score ≥ 14 with < 100 applicants → Apply immediately
- ⭐ **MEDIUM** — Score ≥ 12 → Apply within 24–48 hours
- **LOW** — Partial match → Apply if time permits

---

## 🔧 Customisation

### Change keywords (if your skills change)
Edit `RESUME_KEYWORDS` list in `scripts/generate_report.py`

### Change email recipient
Edit `EMAIL_RECIPIENT` in `scripts/generate_report.py`

### Change run time
Edit the cron in `.github/workflows/daily_job_report.yml`:
```yaml
- cron: "0 17 * * *"   # 10:30 PM IST = 17:00 UTC
```
Use [crontab.guru](https://crontab.guru) to calculate your desired UTC time.

### Add more job search URLs
Add entries to `SEARCH_URLS` in `scripts/generate_report.py`

---

## ❓ Troubleshooting

| Problem | Fix |
|---------|-----|
| Email not received | Check spam folder; verify Gmail App Password has no spaces |
| Apify run fails | Check your Apify account has credits; verify API token in secrets |
| Workflow not triggering | Make sure Actions are enabled in repo settings |
| 0 jobs found | LinkedIn may have changed structure; re-run manually or check Apify actor status |

---

## 📅 Schedule Reference

| Time | Timezone |
|------|----------|
| 10:30 PM | IST (India Standard Time) |
| 5:00 PM  | UTC |
| 12:00 PM | EST |

---

*Built with: Python · openpyxl · Apify LinkedIn Scraper · GitHub Actions · Gmail SMTP*
