## Action center

The Action center is your "single pane of glass" in the Microsoft Defender portal. It consolidates all remediation actions across Defender for Endpoint and Defender for Office 365 into one location.

### Pending

This tab shows investigations that need your attention. You will see recommended actions that you must approve or reject. (This tab is hidden if there are no pending actions).

### History

This is the audit log of everything that has already happened. It tracks:

- Automated investigation remediations.
    
- Actions approved by your security team.
    
- Commands run during Live Response sessions.
    
- Remediation actions applied by Microsoft Defender Antivirus.
    

## Review pending actions

To handle pending items:

1. Go to the **Pending** tab.
    
2. Select an item.
    
3. Open the panel to review details (file info, alert details, etc.) and choose to **Approve** or **Reject** the action. You can also select multiple items to batch-approve or reject.
    

## Review completed actions

To see what’s already done:

1. Select the **History** tab.
    
2. (Optional) Expand the time filter to see more data.
    
3. Click an item to view the full details.
    

## Undo completed actions

If you determine an action was a mistake (e.g., a file wasn't actually a threat), you can reverse it.

- **What you can undo:** Automated investigation actions, Defender Antivirus actions, and manual response actions (like isolating a device, quarantining a file, stopping a service, etc.).
    

## Remove a file from quarantine across multiple devices

If a file was incorrectly quarantined, you can fix it for all affected devices at once:

1. Go to the **History** tab.
    
2. Select the file that was quarantined.
    
3. In the side pane, select **Apply to X more instances of this file**, then select **Undo**.
    

## Viewing action source details

The **Action source** column tells you exactly _how_ an action was triggered.

- **Manual:** Triggered by a human (e.g., Manual device action, Manual email action, Manual live response).
    
- **Automated:** Triggered by the system (e.g., Automated device action, Automated email action).
    
- **Other:** Triggered by specific tools like Advanced hunting, Explorer, or API calls.
    

## Submissions

In organizations with Exchange Online, admins can send suspicious emails, URLs, and attachments to Microsoft for deep scanning and analysis.

### What do you need to know before you begin?

- **Roles:** You need the **Security Administrator** or **Security Reader** role.
    
- **Age:** You can submit messages up to 30 days old.
    
- **Throttling:** There are limits on how many submissions you can make (e.g., 150 per 15 minutes).
    

### Report suspicious content to Microsoft

[SS]

1. Go to the **Submissions** page.
    
2. Select the right tab (Emails, Email attachments, or URLs).
    
3. Select the **Submit to Microsoft for analysis** icon.
    
4. Follow the flyout prompts to submit the content.
    

### Notify users from within the portal

You can inform users about the result of their reports:

1. Go to the **User reported messages** tab.
    
2. Select the message.
    
3. Use the **Mark as and notify** dropdown to mark it as **No threats found**, **Phishing**, or **Junk**. This automatically emails the user with the result.
    

### Submit a questionable email to Microsoft

[SS]

1. Select **Email** in the submission type.
    
2. Use the **Network Message ID** or upload the `.eml`/`.msg` file.
    
3. Choose a recipient to check policy hits against.
    
4. Select a reason (**False positive** or **False negative**). If False negative, tell Microsoft what it _should_ have been (Phish, Malware, or Spam).
    
5. **Submit**.
    

### Send a suspect URL to Microsoft

1. Select **URL** in the submission type.
    
2. Paste the full URL.
    
3. Select the reason (**False positive** or **False negative**) and classify it as Phish or Malware if needed.
    
4. **Submit**.
    

### Submit a suspected email attachment to Microsoft

1. Select **Email attachment** in the submission type.
    
2. Upload the file.
    
3. Select the reason (**False positive** or **False negative**) and classify it.
    
4. **Submit**.
    

### View admin submissions to Microsoft

You can sort and filter your submission history. You can customize the view to see columns like **Status**, **Result**, **Sender**, and **Submission ID**.

### Admin submission result details

Once Microsoft reviews your submission, results appear in the flyout:

- **Authentication:** Did the sender pass email auth?
    
- **Policy hits:** Did your organization's policies override the filter?
    
- **Detonation/Grader analysis:** Results of scanning files/URLs and feedback from human graders. (Note: Grader feedback can take up to a day).
    

### View user submissions to Microsoft

This tab shows what your users have reported via add-ins (like Report Phishing). You can sort and filter these just like admin submissions.

### Undo user submissions

There is no "undo" button for a user submission. However, if a user needs an email back, they can recover it from their own **Deleted Items** or **Junk Email** folders.

### Converting user reported messages from the custom mailbox into an admin submission

If you intercept user reports in a custom mailbox, you can select them from the **User reported messages** tab, click **Submit to Microsoft for analysis**, and officially report them as Clean, Phishing, Malware, or Spam.