# ULTRA SCRAPER v3 — Google Maps Lead Extractor

> 💡 **Community Project**: This tool is open-sourced and provided for free as part of the automation initiatives by **[Apex Automation Team](https://apexautomationteam.com/)**. Visit our official website for enterprise-level automation, custom scraping tools, and AI workflows.

A high-performance, in-browser Google Maps scraper that extracts full business listing datasets directly into a clean, structured `.xlsx` (Excel) spreadsheet with segmented addresses (Street, City, State, Zip, Country), phone numbers, direct website links, and social profiles.

---

## 📺 Video Demos & Tutorials

You can download and watch the full video guides from our official release assets:

* 📥 **[Download Google Maps Scraper Walkthrough (.mp4)](https://github.com/ApexAutomationTeam/Google-Maps-Scraper/releases/download/v1.0.0/Google.Maps.Scraper.mp4)**
* 📥 **[Download Maps Reload & Pagination Tutorial (.mp4)](https://github.com/ApexAutomationTeam/Google-Maps-Scraper/releases/download/v1.0.0/Google.Maps.Scraper-Reload.more.or.again.to.get.more.results.if.low.results.fetched.at.once.mp4)**
* 📥 **[Download Google Drive Scraper Setup Video (.mp4)](https://github.com/ApexAutomationTeam/Google-Maps-Scraper/releases/download/v1.0.0/Drive.Scraper.mp4)**
* 📦 **[View Full Release v1.0.0](https://github.com/ApexAutomationTeam/Google-Maps-Scraper/releases/tag/v1.0.0)**

---

## Repository Files

| File | Description |
| :--- | :--- |
| `G_ULTRA_v3.js` | Source code to paste directly into Chrome DevTools Console. |
| `G_ULTRA_v3_code.txt` | Raw text alternative (prevents browser download blocking). |
| `README.md` | Complete documentation and user guide. |

---

## 1. Quick Start Guide (4 Steps)

1. Open **Google Maps** in Google Chrome and enter your target search query (e.g., `cafes in Lahore`, `dentists in Chicago`, `gyms in London`). Make sure the search results appear on the left panel.
2. Press **`F12`** (or `Right-Click` → `Inspect`) and navigate to the **Console** tab.
3. Open `G_ULTRA_v3_code.txt`, select all (`Ctrl+A`), copy (`Ctrl+C`), paste it into the console, and press **Enter**.
4. Wait 20–60 seconds. The `.xlsx` workbook will automatically download once scanning is complete.

> *Note: If Chrome prevents pasting for the first time, type `allow pasting` into the console, hit Enter, and then paste the code.*

---

## 2. ⚠️ How to Handle New Searches (Important)

When you type a new keyword into the Maps search input box and press Enter, Google Maps frequently delivers a partial "preview" batch containing only **2–3 results**. Furthermore, refreshing the page with **F5 does not solve this**, as Maps often retains the previous state without updating the URL parameter.

### Recommended Workarounds:
* **Option A (Automatic Self-Correction):** Run the script normally. If an incomplete preview is detected, the script will automatically redirect the browser to the full search view and display: *"Paste the script again in 3–4 seconds."* Pasting it a second time retrieves the entire dataset.
* **Option B (Direct URL Search):** Navigate directly via the address bar using the clean pattern:  
  `https://www.google.com/maps/search/gyms+in+Multan`  
  This forces a clean page load and fetches full results immediately.

| Search Method | Average Leads Captured |
| :--- | :--- |
| Standard Search Box (unrefreshed preview) | **3** ❌ |
| Auto-fix applied → Re-paste | **Full Count** ✅ |
| Direct Address Bar URL | **Full Count** ✅ |

---

## 2b. Understanding Lead Variations Across Zoom Levels

Google Maps dynamically updates result sets depending on the user's viewport, map bounding box, and zoom factor.

* **Zoom `10z`** (Regional view): ~154 results
* **Zoom `12z`** (Default city radius): ~155 results
* **Zoom `15z`** (Tight neighborhood focus): ~168 results

To ensure reliable, consistent outputs across repeated runs, launch the clean search path from the address bar without custom `@lat,lng,zoom` coordinates:  
`https://www.google.com/maps/search/cafes+in+lahore`

---

## 3. Excel Output Structure

| Column Header | Description | Example |
| :--- | :--- | :--- |
| `Keyword` | Targeted search query | `cafes in Lahore` |
| `Business Name` | Cleaned entity name | `Arcadian Café` |
| `Category` | Primary Google category | `Cafe` |
| `Rating` | Average user rating | `4.4` |
| `Reviews` | Review count | `9412` |
| `Street` | Normalized street/area line | `Sir Syed Rd, Block K Gulberg 2` |
| `City` | Detected city name | `Lahore` |
| `State` | State or administrative region | `Punjab` |
| `Zip` | Postal / Zip code | `54660` |
| `Country` | Country name | `Pakistan` |
| `Phone` | Clean standard phone digits (CRM / dialer ready) | `345-845-6753` |
| `Phone (Original)` | International format with country code | `+92 345 8456753` |
| `Website` | Official business homepage link | `https://cafezouk.com` |
| `Socials` | Dynamic columns (`Facebook`, `Instagram`, `WhatsApp`, etc.) | Direct profile URLs |
| `Plus Code` | Google Plus Code identifier | `GC29+FQH` |
| `Hours` | Current operating status | `Closes 2 AM` |
| `Price` | Price segment indicator | `Rs 2,000–3,000` |
| `Google Maps URL` | Direct permanent place link via `place_id` | `https://maps.google.com/?cid=...` |
| `Full Address` | Complete concatenated address | Full single-line address |

---

## 4. Configurable Parameters

You can customize these options at the top of the script:

```javascript
const MAX_PAGES  = 15;     // Maximum pages to scan (15 pages x 20 = ~300 leads)
const PAGE_DELAY = 1200;   // Interval between page requests in milliseconds
const FORCE_CITY = '';     // Enforce a hardcoded city name if auto-detection misses




5. Architectural Improvements (v3)
Direct Backend API Interception: Instead of slow UI scraping, clicking elements, or simulating scrolling, the script taps into underlying network data feeds, making it up to 10x faster.

Direct Schema Parsing: Coordinates, ratings, phone numbers, and addresses are extracted directly from structured indices rather than unreliable regular expressions.

Automated Data Quality Validation: Validates domains (filters out Google CDN assets, StreetView thumbnails, and aggregators), validates phone patterns, and rejects internal system tokens.

Native .xlsx Generation: Uses a self-contained in-memory XLSX builder that works without external CDN dependencies or security CSP violations.

6. Troubleshooting
Script outputs only 2–3 results: This occurs when Google Maps only returns a preview batch. Let the script redirect the page, wait 3–4 seconds, and re-paste the code.

Chrome blocks downloading .js: Download and open G_ULTRA_v3_code.txt instead.

Category mismatch: Categories are pulled directly from Google Maps records; use Excel's auto-filter column to narrow them down.

Blank phone numbers: The business owner has not listed a public phone number on their profile.

🏢 About Apex Automation Team
We build custom integrations, web scrapers, automated workflows, and AI solutions to scale your business operations.

Official Website: https://apexautomationteam.com/

Get in Touch: Reach out through our portal for bespoke scraping tools and workflow architectures.
