# AI Resume/CV Parser

> Paste any resume. Get structured data in seconds.
> **Powered by Google Gemini AI. Logs to Google Sheets automatically.**

**Live demo:** [ai-resumecv-parser.netlify.app](https://ai-resumecv-parser.netlify.app)
**Built by:** Dev Pathak · [linkedin.com/in/devpathak18](https://linkedin.com/in/devpathak18) 

---

## What Problem Does This Solve?

HR teams and hiring managers read hundreds of resumes manually every week. The same information — name, email, skills, experience, education — presented in hundreds of different formats. It is repetitive, slow, and error-prone.

**This system eliminates that entirely.**

Paste any resume — any format, any style — and the AI extracts every field, structures the data, and logs it to a Google Sheet automatically. What takes a human 5-10 minutes per resume takes this system under 10 seconds.

---

## How It Works — The Full Pipeline

```
User pastes resume text
        |
        v
┌─────────────────────────┐
│  Netlify Frontend        │  ← JavaScript web form
│  ai-resumecv-parser      │    User hits "Parse Resume"
└────────────┬────────────┘
             |
             v  (POST request with resume JSON)
┌─────────────────────────┐
│  Make.com Webhook        │  ← Automation trigger
│  Receives resume text    │    Scenario activates instantly
└────────────┬────────────┘
             |
             v  (structured prompt + resume content)
┌─────────────────────────┐
│  Google Gemini 2.0 Flash │  ← AI model
│  Reads full resume       │    Extracts 13 data fields
│  Returns structured JSON │    Writes professional summary
└────────────┬────────────┘
             |
        ┌────┴────┐
        v         v
┌──────────┐  ┌──────────────────────┐
│  Google  │  │  Webhook Response     │
│  Sheets  │  │  Returns JSON to      │
│  New row │  │  frontend instantly   │
│  logged  │  │                      │
└──────────┘  └──────────────────────┘
                        |
                        v
              ┌─────────────────┐
              │  Frontend       │
              │  Displays result│
              │  in sections    │
              │  with skill tags│
              │  + score        │
              └─────────────────┘
```

**In plain English:**
1. You paste a resume
2. Make.com catches it and sends it to Gemini AI
3. Gemini reads the resume and extracts everything into JSON
4. Google Sheets gets a new row with all the candidate data
5. The website shows you the structured result instantly

---

## What Gets Extracted

Gemini AI extracts **13 fields** from any resume:

| Field | What It Contains | Example |
|---|---|---|
| `name` | Full candidate name | Dev Pathak |
| `email` | Email address | dev@email.com |
| `phone` | Phone number | +91 ********** |
| `location` | City and country | Bangalore, India |
| `linkedin` | LinkedIn profile URL | linkedin.com/in/devpathak18 |
| `github` | GitHub profile URL | github.com/Devpathak18 |
| `portfolio` | Portfolio website | www.example.com |
| `summary` | AI-written 2-3 sentence professional summary | Generated automatically |
| `skills` | Array of skills | Python, SQL, Gemini AI, Make.com |
| `experience` | Array of jobs with title, company, duration | AI Intern at XYZ Company |
| `education` | Array of degrees with institution and grade | B.Tech CSBS, Jain University |
| `certifications` | Array of certs with issuer and date | Gemini for Google Cloud, Google, 2026 |
| `projects` | Array of projects with description and link | AI Email Automation, github.com/... |

**Sample output:**

```json
{
  "name": "Dev Pathak",
  "email": "devpathakpersonal@gmail.com",
  "phone": "+91 **********",
  "location": "Bangalore, India",
  "linkedin": "linkedin.com/in/devpathak18",
  "github": "github.com/Devpathak18",
  "portfolio": "wwww.example.com",
  "summary": "B.Tech CSBS student at Jain University building AI automation systems independently. Has deployed 4 live AI projects including email automation, research pipelines, and computer vision systems.",
  "skills": ["Python", "Make.com", "Google Gemini AI", "PyTorch", "SQL", "JavaScript"],
  "experience": [
    {
      "title": "AI Automation Developer",
      "company": "Independent",
      "duration": "May 2026 - Present",
      "description": "Built 4 live AI automation systems deployed in production"
    }
  ],
  "education": [
    {
      "degree": "B.Tech Computer Science and Business Systems",
      "institution": "Jain Deemed-to-be University",
      "year": "2028",
      "grade": "8.00 CGPA"
    }
  ],
  "certifications": [
    {
      "name": "Gemini for Google Cloud",
      "issuer": "Google",
      "date": "March 2026"
    }
  ],
  "projects": [
    {
      "name": "AI Email Automation Pipeline",
      "description": "Gmail to Gemini AI to Google Sheets to Telegram",
      "link": "github.com/Devpathak18/email-ai-automation"
    }
  ]
}
```

---

## Live Features on the Frontend

### Structured Sections
Every extracted field is displayed in a clean organised section — not a raw JSON dump. Each section has an icon, title, and the extracted data formatted for easy reading.

### Skill Tags
Skills are displayed as visual pill-shaped tags — the same way LinkedIn and job portals show them. Easy to scan at a glance.

```
[Python]  [Make.com]  [Gemini AI]  [PyTorch]  [SQL]  [JavaScript]
```

### Profile Completeness Score
After extraction, the system calculates what percentage of the 13 fields were successfully found in the resume. Shows as a progress bar.

```
Profile Completeness: 92% — 12 of 13 fields found
```

### AI-Generated Summary
Even if the resume has no summary section, Gemini AI writes a professional 2-3 sentence summary based on the rest of the content.

---

## Tech Stack

| Layer | Tool | Why This Tool |
|---|---|---|
| **Frontend** | HTML + CSS + JavaScript | Lightweight, no framework needed, deploys anywhere |
| **Hosting** | Netlify | Free static hosting, HTTPS, instant deploy by drag and drop |
| **Automation** | Make.com | No-code workflow orchestration — connects all services via webhooks |
| **AI Model** | Google Gemini 2.0 Flash | Fast, accurate, free tier handles 1,500 requests per day |
| **Storage** | Google Sheets | Free, shareable, searchable HR database out of the box |
| **API** | Gemini API via Make.com | No backend server needed — Make.com makes the API call |

**Why no backend server?**
The entire system uses Make.com as the middleware. The HTML frontend sends a webhook to Make.com → Make.com calls Gemini API → Make.com logs to Sheets → Make.com responds to the frontend. Zero server management, zero deployment complexity.

---

## Repository Structure

```
ai-resumecv-parser/
├── README.md                  ← You are here
├── ai-resume-parser.html      ← Complete frontend (one file)
├── make-blueprint.json        ← Import into Make.com (one click)
└── .env.example               ← Configuration reference
```

---

## Setup Guide

### What You Need
- Make.com account (free tier works)
- Google account (for Gemini API + Google Sheets)
- Netlify account (free — for hosting)
- Gemini API key (free — from aistudio.google.com)

### Step 1 — Get Gemini API Key
1. Go to **aistudio.google.com**
2. Click **Get API Key** → **Create API Key**
3. Copy the key — keep it safe

### Step 2 — Set Up Google Sheets
Create a new Google Sheet with these column headers in Row 1:
```
Timestamp | Name | Email | Phone | Location | Skills | Full JSON
```

### Step 3 — Import Make.com Blueprint
1. Go to **make.com** → Create New Scenario
2. Click the three dots menu → **Import Blueprint**
3. Select `make-blueprint.json` from this repo
4. Update the Google Sheets module → connect your Google account → select your sheet
5. Update the Gemini module → connect your Google account
6. Copy the webhook URL from Module 1

### Step 4 — Configure Frontend
Open `ai-resume-parser.html` → find this line:
```javascript
const WEBHOOK_URL = 'YOUR_MAKE_WEBHOOK_URL';
```
Replace with your Make.com webhook URL.

### Step 5 — Deploy to Netlify
1. Go to **netlify.com**
2. Drag and drop `ai-resume-parser.html`
3. Get your live URL instantly

### Step 6 — Turn On and Test
1. In Make.com → toggle scenario **ON**
2. Go to your Netlify URL
3. Paste any resume text
4. Click **Parse Resume**
5. Watch the result appear and check your Google Sheet for the new row

---

## Challenges I Faced and Fixed

### Challenge 1 — HTTP Module JSON Error
**Problem:** Make.com's raw HTTP module kept throwing `InvalidConfigurationError` for the JSON body containing the Gemini prompt with special characters.

**Fix:** Switched from the raw HTTP module to Make.com's native **Google Gemini module** — it handles all JSON serialisation internally without manual body formatting.

### Challenge 2 — CORS Error on Local File
**Problem:** Opening the HTML file locally (`file://`) caused browsers to block the fetch request to Make.com's webhook.

**Fix:** Deployed to Netlify — requests from HTTPS domain pass CORS checks without any server-side headers needed.

### Challenge 3 — Wrong Gemini Model Name
**Problem:** `gemini-1.5-flash` returned a 404 — model not found in API version v1beta.

**Fix:** Updated to `gemini-2.0-flash` — the current supported model as of 2026.

### Challenge 4 — Module ID Reference Error
**Problem:** After switching from HTTP to Gemini module, all downstream modules were still referencing the old module ID number.

**Fix:** Remapped all variable references using Make.com's variable picker to the correct new module IDs.

---

## Cost to Run This

| Service | Free Tier | Cost |
|---|---|---|
| Make.com | 1,000 operations/month | Rs. 0 |
| Google Gemini API | 1,500 requests/day | Rs. 0 |
| Google Sheets | Unlimited rows | Rs. 0 |
| Netlify | 100GB bandwidth/month | Rs. 0 |
| **Total** | **1,500 resumes/day** | **Rs. 0** |

---

## Related Projects

This is Project 4 in an ongoing AI automation series:

| Project | What It Does | Link |
|---|---|---|
| AI Email Automation | Gmail → Gemini AI → Google Sheets → Telegram | [github.com/Devpathak18/email-ai-automation](https://github.com/Devpathak18/email-ai-automation) |
| AI Research Pipeline | Any topic → Local LLM → 7-step guide → Google Docs | [github.com/Devpathak18/ai-research-pipeline](https://github.com/Devpathak18/ai-research-pipeline) |
| AI Product Intelligence | CLIP-based reverse search + duplicate detection + recommendations | [github.com/Devpathak18/ai-product-intelligence-clip](https://github.com/Devpathak18/ai-product-intelligence-system) |
| **AI Resume Parser** | **Resume text → Gemini AI → structured data → Google Sheets** | **This repo** |

---

## Author

**Dev Pathak**
B.Tech Computer Science and Business Systems (CSBS)
Program co-designed with Tata Consultancy Services

- GitHub: [github.com/Devpathak18](https://github.com/Devpathak18)
- LinkedIn: [linkedin.com/in/devpathak18](https://linkedin.com/in/devpathak18)
- Email: devpathakpersonal@gmail.com

*Open to remote internships in AI automation, data analytics, and machine learning.*

---

## Star This Repo

If this project helped you or gave you ideas — star the repo. It helps others find it.
