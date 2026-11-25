# Prepare for App Store · a Claude Code skill

> A [skill](https://docs.claude.com/en/docs/claude-code/skills) for [Claude Code](https://claude.com/claude-code) that turns your Xcode project into a ready-to-submit App Store listing.

**Your app is finished. Now App Store Connect wants a name, a subtitle, 100 characters of keywords, a 4,000-character description, privacy answers, an age rating, screenshots in exact pixel sizes, review notes, and a screenshot for every in-app purchase.**

This skill does that last mile for you. Install it once, open your Xcode project in Claude Code, say *"prepare this app for the App Store"*, and you get one folder with every field written, measured and ready to paste.

---

## Why you'd want it

- **Get found, not just listed.** Only the Name, Subtitle and Keywords are searchable on the App Store. The skill looks at the terms people actually type, checks what competing apps use (through Apple's public iTunes Search API), scores each term on relevance, search volume and competition, and spreads the best ones across those three fields without wasting a character on repeats.
- **Screenshots that sell.** Each screenshot headline answers the problem the user searched for ("See where your money goes", not "Statistics screen"). The images use real captures from the simulator, a clean light layout, your brand color, and one key UI element that pops out of the device frame. They come out at Apple's exact sizes with no alpha channel.
- **Avoid common rejections.** It checks your paywall against Guideline 3.1.2 (price, period, auto-renewal text, Restore Purchases button, Terms and Privacy links). It writes App Review notes that walk the reviewer through your app, and makes one review screenshot per IAP and subscription.
- **Nothing made up.** It reads your project (targets, `Info.plist`, `PrivacyInfo.xcprivacy`, StoreKit code, localizations) before writing anything. When it can't verify something, such as prices or a demo account, it asks you instead of guessing.
- **Copy, paste, done.** Every text field sits in its own code block with its character count measured by a script, so you can paste straight into App Store Connect.
- **Leaves no mess.** Temporary files are deleted, the simulator is reset, and `git status` confirms that nothing outside `appstore-prepare/` was touched.

## What you get

```
appstore-prepare/
├── README.md                  # Summary, limit checks, things to confirm, code fixes needed
├── 00-app-analysis.md         # What your app does, for whom, on which platforms
├── 01-app-information.md      # Name, Subtitle, Category, Privacy, Age Rating
├── 02-product-page.md         # Promotional Text, Description, Keywords, URLs, Copyright
├── 03-aso-research.md         # Keyword research, competitor scan, and why each word was picked
├── 04-app-review.md           # Contact info + notes for the App Review team
├── 05-in-app-purchases.md     # Every IAP, Product IDs matched to your code
├── 06-subscriptions.md        # Subscription group, plans, intro offers
├── screenshots/               # 3 final images per platform: iPhone 6.9", 6.3", iPad 13", Mac
└── review-screenshots/        # One paywall capture per IAP / subscription
```

Each locale your app supports gets its own keywords and screenshot headlines, written for what people search in that language rather than translated word for word.

## Install

```bash
git clone https://github.com/musatenekeci/prepare-for-appstore.git ~/.claude/skills/prepare-for-appstore
```

That's it. Claude Code loads every skill in `~/.claude/skills/` automatically, so this one is now available in all your projects.

To install it for a single project instead, clone it into that project's `.claude/skills/` folder.

## Use

In the root of your iOS or macOS project, start Claude Code and say:

```
/prepare-for-appstore
```

or simply *"I want to publish this app on the App Store"*.

The first time, it asks for the few things only you know: your contact details for App Review, your privacy, support and marketing URLs, and the copyright holder. Then it works through the steps on its own.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- macOS with Xcode and the iOS Simulator (used to take real screenshots)
- An iOS, iPadOS or macOS app project

## Good to know

- It prepares your submission; it does not upload anything. You stay in control of what goes into App Store Connect.
- Read the result before you submit. Prices, legal text and privacy answers are your responsibility, and the skill lists everything it couldn't confirm under *Items for the developer to confirm*.
- Apple changes screenshot sizes from time to time. The skill checks the [official spec page](https://developer.apple.com/help/app-store-connect/reference/app-information/screenshot-specifications/) on every run.

## License

[MIT](LICENSE)
