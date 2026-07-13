### What we are learning in this section

In this section, we are learning about the **Security Copilot Threat Intelligence Briefing Agent**. We are moving away from manually gathering threat feeds and trying to piece them together ourselves. Instead, we’re learning how to let AI take the lead, gather the intel, and hand us a finished "briefing" that tells us exactly what threats we need to worry about right now.

### What is this agent doing for me?

Normally, an analyst has to spend hours or even days jumping between different security tools, reading threat feeds, and trying to figure out which threats actually matter to their specific organization. It’s exhausting and often makes you feel like you’re constantly playing catch-up.

The **Threat Intelligence Briefing Agent** acts like your own personal AI assistant. It does the heavy lifting for you—collecting data, correlating it with your internal vulnerabilities, and summarizing the "so what?" in a matter of minutes. It doesn't just read data; it dynamically decides what to analyze next based on what it finds, giving you a tailored report that identifies emerging threats and their potential impact on your business.

### Where do I find this tool?

You don't have to hunt for it in some hidden menu. You can find the agent as a banner right at the top of the **Threat analytics** page in the Microsoft Defender portal. To get there, you just go to **Threat intelligence > Threat analytics** in your navigation menu.
![](images/Pasted%20image%2020260713132652.png)

### What do I need to run this?

You can't just flip a switch without having the right "keys." To get this working, you need a few things set up:

- **The Copilot:** You need Microsoft Security Copilot available in your environment.
    
- **The Plugins:** You must have the **Microsoft Threat Intelligence** plugin enabled (mandatory), and it is highly recommended to also use the **Microsoft Defender External Attack Surface Management (EASM)** and **Vulnerability Management** plugins for the best context.
    
- **The Permissions:** You need the right access levels—typically **Security Reader** access to Threat analytics and **Security Admin** access for the initial setup and configuration.
    

### How do I read these reports?

Once the agent finishes its work, it drops a full briefing panel for you. This panel isn't just a wall of text; it summarizes the threats, shows you your vulnerable exposures (what's broken that might get you hacked), and explains the potential business impact. You don't have to keep it inside the portal, either—you can copy the text or download the whole thing as a **markdown file** to share with your team or bosses.
![](images/Pasted%20image%2020260713132706.png)
![](images/Pasted%20image%2020260713132710.png)

### How do I manage and improve this AI?

The agent isn't a "set it and forget it" tool. You can go into **Manage agent** settings to update how often it runs (the schedule) and adjust your briefing preferences.

If you want to look at old reports, you can find them in the Security Copilot **Activity** area. And here’s the cool part: you can use a "thumbs up" or "thumbs down" to tell the AI if the report was helpful. This feedback loop is how you make the agent smarter so that future briefings are even more relevant to your organization.

![](images/Pasted%20image%2020260713132719.png)
![](images/Pasted%20image%2020260713132724.png)![](images/Pasted%20image%2020260713132728.png)![](images/Pasted%20image%2020260713132740.png)
### What is coming up next

You’ve now mastered how to hunt for threats, investigate identity sign-ins, and generate AI-powered intelligence briefings. In the next section, we will be **Analyzing reports**, where we look at how to synthesize all of this intelligence into actionable defense strategies for your entire organization.

[Threat Intelligence Briefing Agent in Microsoft Defender](https://www.youtube.com/watch?v=86nfFsaBSfk)

This video provides a practical look at how the Threat Intelligence Briefing Agent works within the Microsoft Defender portal.