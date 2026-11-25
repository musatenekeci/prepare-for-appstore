---
name: prepare-for-appstore
description: Prepare an iOS/macOS app for App Store release — app analysis, ASO keyword research, App Store Connect metadata (name, subtitle, keywords, description, privacy, age rating), screenshots, App Review notes, in-app purchases and subscriptions, all written into an appstore-prepare/ folder. Use when the user wants to publish/submit an app to the App Store or prepare App Store Connect listing content.
---

# App Store Release Preparation

We are about to publish this app on the App Store. This file describes every field that must be filled in App Store Connect and how each one should be prepared. Follow the steps in order.

### Developer details

Several fields need the developer's own details. Before writing any file, ask the user for the ones you don't already know (from the conversation, the project, or an existing `appstore-prepare/` folder) in a single question, and never invent them:

- Contact for App Review: first name, last name, phone, email
- Privacy Policy URL, Support URL, Marketing URL (optional)
- Copyright holder (person or company name)

If the user skips any of them, leave a clear `TODO` in the relevant file and list it under "Items for the developer to confirm" in `README.md`.

---

## 0. Before You Start

### 0.1 Create the folder

Create a folder named `appstore-prepare/` in the project root and split the output into this structure:

```
appstore-prepare/
├── README.md                  # Summary + pre-upload checklist
├── 00-app-analysis.md         # App analysis (see 0.2)
├── 01-app-information.md      # Name, Subtitle, Category, Privacy, Age Rating
├── 02-product-page.md         # Promotional Text, Description, Keywords, URLs, Copyright
├── 03-aso-research.md         # Keyword research and rationale
├── 04-app-review.md           # Contact information + App Review Notes
├── 05-in-app-purchases.md     # IAP products (write "None" if there are none)
├── 06-subscriptions.md        # Subscriptions (write "None" if there are none)
├── screenshots/
│   ├── iphone-6.9/            # 01.png, 02.png, 03.png
│   ├── iphone-6.3/
│   ├── ipad-13/               # Only if iPad is supported
│   └── mac/                   # Only if Mac is supported
└── review-screenshots/        # Only if there are IAPs or subscriptions
    ├── in-app-purchases/      # <product-id>.png, one per IAP
    └── subscriptions/         # <product-id>.png, one per subscription
```

The `screenshots/` and `review-screenshots/` folders contain **only the final images**. Raw captures, templates, scripts and any other intermediate files are kept in a temporary directory outside the project (e.g. `/tmp/appstore-work/`) and deleted at the end (see section 9).

Put every text field in its own code block so it can be copied straight into App Store Connect, and note its character count next to it.

### 0.2 Analyze the app (`00-app-analysis.md`)

Before writing any copy, inspect the project and extract:

- **Platforms:** Xcode targets, `TARGETED_DEVICE_FAMILY` (1 = iPhone, 2 = iPad), `SUPPORTS_MACCATALYST`, is there a separate macOS target?
- **The problem the app solves:** What need makes a user search for this app? (A list of needs, not a list of features.)
- **Core features:** The 3–5 most valuable features, ranked by value.
- **Audience and languages:** Supported localizations (`.xcstrings`, `Localizable.strings`).
- **Purchases:** StoreKit code, `.storekit` configuration file, product IDs.
- **Data collection:** Analytics, crash reporting, accounts/sign-in, location, camera, photos, health, etc. (`Info.plist` usage descriptions, `PrivacyInfo.xcprivacy`, third-party SDKs).
- **Is sign-in required?** If so, a demo account is needed for review.
- **Brand color and font:** Accent color in the asset catalog, fonts used in the app.

This analysis is the foundation for every field below. Never invent information you are unsure of; list it in `README.md` under "Items for the developer to confirm".

---

## 1. ASO Strategy (do this first, `03-aso-research.md`)

Name, Subtitle and Keywords are the **only** text fields indexed by App Store search. Description and Promotional Text are not indexed. These three fields must be prepared with the greatest care.

### 1.1 Research

1. Based on the problem the app solves, list **20–40 candidate terms** a user would type into the App Store search bar (short-tail + long-tail, e.g. "budget", "expense tracker", "monthly spending").
2. Study competitors: for each main term, use the iTunes Search API (`https://itunes.apple.com/search?term=<term>&country=us&entity=software&limit=15`, repeat with the relevant `country` code for each locale) to pull the `trackName`, `description` and genre of the top-ranking apps. Note the words that recur in competitors' names and subtitles.
3. Score every candidate term on three axes and present them in a table:
   - **Relevance:** Does the app truly satisfy this search? (Irrelevant term = low conversion = lost ranking.)
   - **Estimated volume:** How general / frequently searched is the term?
   - **Competition:** Are there strong brands in the top 10 results?
4. The best opportunity for a new app: terms with **medium volume + low competition + high relevance**. Don't rely on very generic terms dominated by giants (e.g. "photo" or "notes" on their own).

### 1.2 Placement rules

- Apple **combines** the words in Name + Subtitle + Keywords for indexing. **Never repeat** a word across these three fields; repeating it wastes characters.
- Most valuable keyword → **Name**, second tier → **Subtitle**, the rest → **Keywords**.
- Write words individually; Apple builds the combinations itself (if "expense" and "tracker" both exist, the search "expense tracker" is also matched).
- Prepare a separate ASO set for each supported locale; not a literal translation, but the terms people actually search for in that language.

---

## 2. App Information (`01-app-information.md`)

### Name (max 30 characters)

- Format: `Brand – Main Keyword` or `Brand: Main Keyword`. E.g. `Fokus: Pomodoro Timer`.
- The main keyword must be the **most valuable** term from the research.
- Use the 30 characters wisely without hurting readability; Apple may reject keyword stuffing.
- Propose 2–3 alternatives and mark one as "recommended".

### Subtitle (max 30 characters)

- A short phrase that describes the benefit and contains **second-tier keywords** not used in the Name. E.g. `Focus, study, get things done`.
- Describe the **outcome**, not the feature ("Split bills in 1 minute", not "AI-powered app").
- Do not repeat any word from the Name.
- Propose 2–3 alternatives and mark one.

### Category

- **Primary Category =** The category that best matches the app's main function. This is where you compete in search and charts; prefer the relevant one with lower competition.
- **Secondary Category =** The second best fit (optional).
- Note: In App Store Connect, subcategories exist only for Games (and Stickers); for other apps the equivalent is the Secondary Category.
- Explain the choice in 1–2 sentences.

### Privacy Policy URL (required)

- **Privacy Policy URL =** the URL from the developer details (e.g. `https://example.com/privacy`)
- If no page exists, draft a privacy policy based on the data collection findings from 0.2.

### App Privacy (Data Collection)

Based on the 0.2 analysis, list the answers to the "App Privacy" questions in App Store Connect: which data types are collected, whether they are linked to the user, and whether they are used for tracking. If no data is collected, write "Data Not Collected".

### Age Rating

Write the answers to Apple's age rating questionnaire for this app and the expected resulting age rating.

---

## 3. Product Page Information (`02-product-page.md`)

### Promotional Text (max 170 characters)

- Appears above the Description, is **not indexed** in search, and can be updated at any time without submitting a new version.
- State the strongest benefit or a current piece of news (new feature, campaign) in one fluent sentence.
- E.g. `See your weekly spending at a glance. New: shared budgets let you split costs with roommates instantly.`

### Description (max 4000 characters)

Not indexed in App Store search, but it affects conversion; users see only the first ~3 lines before tapping "more". Structure:

1. **First 1–3 lines (hook):** Name the user's problem and give the solution in one sentence.
2. **Benefit blocks:** 3–5 short paragraphs or bullets; each describes one need and the app's answer to it.
3. **Feature list:** Short bullets (•).
4. **Subscription info (if any):** Plan names, durations, auto-renewal explanation, cancellation info.
5. **Links:** Privacy policy and (if there are subscriptions) Terms of Use / EULA link — Apple requires this for subscription apps. Standard EULA: `https://www.apple.com/legal/internet-services/itunes/dev/stdeula/`

Rules: No keyword stuffing, no competitor or brand names, no unverifiable claims ("#1 app").

### Keywords (max 100 characters)

- Comma-separated, **no space after commas**: `budget,expense,spending,savings,bill`
- Do **not repeat** words used in the Name and Subtitle.
- The singular form is enough; don't include both singular and plural of the same word.
- Don't include these (wasted characters): `app`, `free`, `iphone`, the category name, competitor brand names.
- Fill all 100 characters; use any remaining space for the next best terms from the research.
- In `03-aso-research.md`, explain in one line why each word was chosen.

### URLs and Copyright

- **Support URL =** from the developer details (required)
- **Marketing URL =** from the developer details (optional)
- **Copyright =** current year + rights holder, e.g. `2026 Example Inc.` (Apple format: year + rights holder)

---

## 4. App Store Screenshots (`screenshots/`)

### 4.1 Sizes

Produce **3** images for each platform the app supports. Verify the current sizes every time on this page:
https://developer.apple.com/help/app-store-connect/reference/app-information/screenshot-specifications/

| Platform | When | Size (portrait) |
|---|---|---|
| iPhone 6.9" (Dynamic Island, large) | Always | 1320 × 2868 |
| iPhone 6.3" (Dynamic Island, medium) | Always (the size Apple marks as required) | 1206 × 2622 |
| iPad 13" | If iPad is supported | 2064 × 2752 |
| Mac (16:10) | If Mac is supported | 2880 × 1800 |

Format: PNG or JPEG, **no alpha channel / transparency** (RGB, solid background).

### 4.2 Message strategy: answers, not features

The user is searching the App Store for a solution to a problem. Each image must be the answer to that problem; it must not merely introduce the app.

- Each image's headline is a 3–6 word sentence that **directly answers** the need the user is searching for.
  - ❌ `Statistics screen` / `Advanced charts`
  - ✅ `See where your money goes`
  - ❌ `Pomodoro timer`
  - ✅ `Put the phone down, get it done`
- Where possible, work the main ASO keywords naturally into the headlines; when searchers see the word they typed in the image, they think "this is what I was looking for".
- An optional one-line subheadline; nothing more.
- **Order (the first 3 images appear in search results — the most critical area):**
  1. The main answer to the user's core problem (the most searched need).
  2. The second strongest benefit.
  3. The differentiator / ease / trust ("No ads", "No account needed", "One tap", etc.).
- Headlines must be in the locale's language; produce a separate set for each language.

### 4.3 Visual design rules

- **Light theme:** White or very light neutral background (e.g. `#FFFFFF`, `#F6F7F9`). The app's own screen captures must also be taken in light mode.
- **Minimalist:** One message and one highlight per image. No decorations, icon clutter or busy backgrounds.
- **Single accent color:** The app's brand color; use it to highlight one word in the headline or the pop-out element.
- **Typography:** Bold, large headline (readable even at thumbnail size), max 2 lines. SF Pro or the app's own font.
- **Layout:** Headline on top, device below; the device may be slightly cropped at the bottom. All 3 images share the same layout (a series feel).
- **Pop-out effect (the most important rule):** In every image, **one UI element** the user cares about for that need (a button, card, number, list row, chart) must break **outside** the device frame:
  - Crop the element from the real screen capture (or recreate it exactly).
  - Scale it to 115–130% and position it so it extends beyond the edge of the device.
  - Give it a soft, wide shadow (e.g. `0 30px 60px rgba(0,0,0,.12)`); it may sit at a slight angle.
  - The screen inside the device keeps showing the context of the popped-out element.
- **Real interface:** Apple expects screenshots to show the app in use. The popped-out element and the screen must be UI that actually exists in the app.
- **Sample data:** Screens must not be empty; fill them with realistic, friendly sample data.

### 4.4 Production

1. Run the app in the simulator in light mode with sample data and capture raw screens to the temporary directory: `xcrun simctl io booted screenshot /tmp/appstore-work/raw-01.png`. Clean the status bar first: `xcrun simctl status_bar booted override --time 9:41 --batteryLevel 100 --cellularBars 4`.
2. For Mac, capture the app window with `screencapture -l <windowID>`.
3. Compose each final image (background, headline, device frame, pop-out element) at the **exact pixel size** from 4.1, using whatever tooling you prefer; keep any intermediate files in the temporary directory.
4. Save only the final images into `appstore-prepare/screenshots/<platform>/` as `01.png`, `02.png`, `03.png`.
5. Remove the alpha channel and verify the sizes: `sips -g pixelWidth -g pixelHeight -g hasAlpha <file>`.
6. Check readability by viewing the three images side by side at small (search result thumbnail) size, without saving that preview anywhere in the project.

---

## 5. App Review Information (`04-app-review.md`)

### Contact Information

Use the contact details from the developer details (see the top of this file):

- **First Name:**
- **Last Name:**
- **Phone:** (with country code)
- **Email:**

### Sign-In Information

If the app requires sign-in: a demo account username and password (an account with access to all features that does not expire). If not, the "Sign-in required" box stays unchecked.

### Notes (App Review Note, max 4000 characters)

Write a short note that lets the review team understand the app quickly and test it without friction:

- What the app does (1–2 sentences).
- Step-by-step guidance to test the main flow.
- Why permissions (camera, location, notifications, etc.) are requested.
- If there are purchases/subscriptions: how to reach the paywall, and that it can be tested with sandbox.
- Non-obvious features (e.g. a feature that requires specific hardware, behavior tied to a language/region).

---

## 6. In-App Purchases (`05-in-app-purchases.md`)

Extract from the StoreKit usage in the code and the `.storekit` file. If the app has no one-time purchases, write "None". Otherwise, for each product:

| Field | Description |
|---|---|
| Type | Consumable / Non-Consumable |
| Reference Name | Internal name visible only in App Store Connect (e.g. `Pro Lifetime`) |
| Product ID | **Exactly** the same as in the code (e.g. `com.example.app.pro.lifetime`) |
| Price | Price tier (USD base + equivalents for the app's main markets) |
| Display Name | Name shown to users (max 30 characters, per language) |
| Description | Short description shown to users (max 45 characters, per language) |
| Review Screenshot | `review-screenshots/in-app-purchases/<product-id>.png` (see 6.1) |
| Review Notes | What the product unlocks and how to test it |
| Family Sharing | Recommendation: on / off |

Note: IAPs must be submitted for review together with the first version (added to the version from the "In-App Purchases and Subscriptions" section).

### 6.1 Review Screenshot (one per IAP, required)

Apple requires a review screenshot for every In-App Purchase. It is seen only by the review team, so it is **not** a marketing image: it is a plain, real capture of the app showing where and how the product is purchased.

- Produce **one screenshot per IAP product** and name it after its product ID: `review-screenshots/in-app-purchases/<product-id>.png`.
- It must show the purchase screen (paywall / store screen) with **that specific product** visible, preferably selected, with its display name and price.
- Capture it on the iPhone 6.3" or 6.9" simulator, light mode, clean status bar (same setup as 4.4), using a StoreKit configuration file (`.storekit`) so real product names and prices load.
- No headline, frame, background or pop-out effect; the raw full-screen capture is the deliverable.
- PNG or JPEG, no alpha channel, at least 640 × 920 pixels (a simulator capture easily meets this).
- If the paywall cannot show the product (e.g. products don't load), note it under "Items for the developer to confirm" instead of faking the screen.

---

## 7. Subscriptions (`06-subscriptions.md`)

If the app has no subscriptions, write "None". Otherwise:

**Subscription Group**
- Group Reference Name (e.g. `Premium`)
- Group Display Name (per language)

**For each subscription**

| Field | Description |
|---|---|
| Reference Name | Internal name (e.g. `Premium Monthly`) |
| Product ID | Exactly the same as in the code |
| Duration | 1 week / 1 month / 3 months / 6 months / 1 year |
| Price | Price tier (USD + equivalents for the app's main markets) |
| Group Level | Level ranking (top plan = 1) |
| Display Name / Description | Name and description shown to users (per language) |
| Introductory Offer | Free trial or discounted intro (duration, type) |
| Review Screenshot | `review-screenshots/subscriptions/<product-id>.png` (see 7.1) |
| Review Notes | What is unlocked and how to test it |

### 7.1 Review Screenshot (one per subscription, required)

Every subscription needs its own review screenshot. Follow exactly the same rules as 6.1, with these differences:

- Save as `review-screenshots/subscriptions/<product-id>.png`, one file per subscription product.
- The paywall must show **that subscription plan selected**, with its price, billing period, and auto-renewal text visible. If the paywall lists several plans, take a separate capture for each plan with that plan selected.
- If there is an introductory offer, the offer text (e.g. "7-day free trial, then $4.99/month") must be visible in the capture.

**Required checks (Apple Guideline 3.1.2)**
- Does the paywall clearly show price, duration and auto-renewal information?
- Are Privacy Policy and Terms of Use (EULA) links present both in the app and in the Description?
- Is there a "Restore Purchases" button?

If anything is missing, list it in `README.md` under "Code fixes required before release".

---

## 8. Final Check (`README.md`)

When everything is done, write the following in `README.md`:

1. **Summary:** The chosen Name, Subtitle, Keywords and category at a glance.
2. **Character limit table:** Name ≤30, Subtitle ≤30, Promotional Text ≤170, Description ≤4000, Keywords ≤100, Review Notes ≤4000 — measure the actual count of each field with a script and record it.
3. **Duplicate check:** Confirm that no word is repeated across Name, Subtitle and Keywords.
4. **Screenshot check:** Size, format and absence of alpha channel for every file in `screenshots/` and `review-screenshots/`, and that every IAP and subscription product ID has exactly one review screenshot.
5. **Items for the developer to confirm:** Information that must not be invented and could not be verified (prices, privacy URL, demo account, etc.).
6. **Code fixes required before release:** If any.

---

## 9. Cleanup (last step, mandatory)

When all work is finished, leave nothing unnecessary behind:

1. Delete the temporary working directory and everything in it (`rm -rf /tmp/appstore-work`): raw captures, compositing files, previews, helper scripts (e.g. character-count or image-check scripts).
2. Reset the simulator status bar: `xcrun simctl status_bar booted clear`. Remove any sample data you added to the simulator, and shut down simulators you booted for this task.
3. Make sure no temporary or helper files were created inside the project (outside `appstore-prepare/`) and that no project source files were modified. Check with `git status`; anything you created other than `appstore-prepare/` must be removed.
4. Inside `appstore-prepare/`, keep only the files listed in the structure in 0.1. Remove anything else (e.g. `.DS_Store`, drafts, unused alternative images).
5. If a file could not be deleted, list it in `README.md`.
