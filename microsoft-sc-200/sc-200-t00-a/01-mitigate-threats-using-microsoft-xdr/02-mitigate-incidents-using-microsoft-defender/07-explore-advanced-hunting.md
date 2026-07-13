This is where you level up from a "security guard" watching for alarms to a "digital detective" actively hunting for hidden criminals.

### What we are learning in this section

In this section, you are learning about **Advanced Hunting**. We are moving beyond just looking at the alerts that the system throws at us. You are learning how to dive into the raw, 30-day history of your entire network, write custom queries to find "invisible" threats, turn those queries into auto-detecting rules, and use visual graphs to map out how an attacker moves through your system.

### What is Advanced Hunting?

Think of this as a super-powered search engine for your entire network. Instead of waiting for an alert to pop up, you go looking for trouble yourself. You get access to 30 days of raw data from across your entire environment—Endpoints, Office 365, Cloud Apps, and Identity.

It’s important to know _when_ the data shows up:

- **Event/Activity data (the fast stuff):** Alerts and system events appear almost immediately after the sensors send them to the cloud.
    
- **Entity data (the slow stuff):** Info about users and devices updates every 15 minutes, but gets fully consolidated every 24 hours.
    

### What is the Schema?

If Advanced Hunting is a search engine, the Schema is the map that tells you where to look. It’s a list of tables and columns that hold all your data. You can’t write a good query if you don’t know where the data lives, so you’ll use the "Schema reference" constantly to find table descriptions, the columns available, and examples of how to write the query.

### What are Custom Detections?

Hunting is great, but you don't want to hunt for the same thing 24/7. Custom Detections allow you to take a "hunting query" you wrote and turn it into a rule that runs on autopilot.

- **The Golden Rule:** Every rule is capped at 100 alerts per run to stop you from getting spammed.
    
- **The Requirement:** Your query _must_ return three specific columns to work: `Timestamp`, `DeviceId`, and `ReportId`.
    
- **The Autopilot:** You can set these to run every hour, every 3 hours, or even in "Continuous (NRT)" (Near Real-Time) mode to catch bad guys the second they do something suspicious.
    
- **The Response:** Once the rule catches something, it can automatically Isolate the device, run an AV scan, or Quarantine a file without you lifting a finger.
    

### What is the Hunting Graph?

Sometimes, reading tables of data is mind-numbing. The Hunting Graph is a visual map integration that shows you how everything is connected. Instead of stitching together relationships with complex code, you get a visual picture of "nodes" (entities like users or devices) and "edges" (the relationships between them, like permissions or traffic). It’s perfect for spotting "attack paths"—the literal route a hacker takes to get from Point A to your most sensitive data.

### What is coming up next

In the next section, we are shifting focus to **Microsoft Entra sign-in logs**. This is the identity layer. We are going to look at how to track every single user login, spot brute-force attempts, and differentiate between a normal user logging in and a malicious actor trying to hijack an account.