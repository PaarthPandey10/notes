### What we are learning in this section

In this section, we are learning how to automate the "heads-up" system in the Microsoft Defender portal. You are learning how to configure email notifications so that the system tells _you_ when something big happens, instead of you having to go digging for it. We’re covering the two main types of alerts: **Incidents** (urgent stuff) and **Threat Analytics** (intelligence briefings).

### What tf is this whole "Email Notifications" thing?

Think of this as your "push notification" system. You shouldn't live in the Defender portal; you have a life. By setting up email notifications, you ensure that as soon as the system flags an issue, it hits your inbox instantly. There are two main types of notifications you can configure:

- **Incidents:** Use this when you need to know _immediately_ that a new incident has been created so you can start the response.
    
- **Threat Analytics:** Use this to get notified when new threat intelligence reports are published, so you can stay ahead of the curve.
    

### What tf is the path to set this up?

The path is the same regardless of what you are configuring. You don't need to learn a different maze for every feature. To find these settings, you always start in the same place:

1. Go to the **Settings** page in the navigation pane.
    
2. Select **Microsoft Defender XDR**.
    
3. Click **Email notifications**.
    

From there, you just pick whether you want to configure **Incidents** or **Threat Analytics**.

### How tf do I actually build a notification rule?

The process is so identical for both types that once you learn it, you’ve learned it for everything. It’s a simple 4-step flow:

1. **Add a Rule:** Click "Add incident email notification" (or "Create a notification rule" for analytics).
    
2. **The Basics:** Give it a **Name** and a **Description** so you know what it is later.
    
3. **The Filter:** Pick the **Criteria** (e.g., specific devices or specific alert types) so you don't get spammed with junk you don't care about.
    
4. **The Destination:** Enter the **Email Address** of the person or team that needs to handle this.
    
5. **Finish:** Hit **Create rule** and you're done.
    

It’s really that simple. You’re building a customized alarm system that only rings when you actually need it to.

### What is coming up next

You have now completed the core training on how to mitigate incidents using Microsoft Defender. You’ve gone from monitoring logs and hunting threats to configuring your own automated response system. You’ve got the full picture of the Defender ecosystem—from the initial alert to the final report. Great work getting through the training!