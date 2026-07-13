### What we are learning in this section

In this section, we are learning how to use **KQL (Kusto Query Language)** to specifically hunt through **Microsoft Entra sign-in logs**. You will learn which tables you need to pull from to see if someone’s login was legit or if they got blocked by a security policy.

### What tf are we using to search these logs?

You can't just stare at a dashboard all day; sometimes you need to write a query to find the exact login you care about. Depending on where you are hunting, you use two different tables:

- **If you are in Microsoft Defender XDR Threat Hunting:** You use the `AADSignInEventsBeta` table.
    
- **If you are using Log Analytics:** You use the `SigninLogs` table.
    

It’s the same data, just two different places to grab it depending on which tool you have open.

### What tf do I actually see in the Azure Portal?

If you don't want to write a code query and just want to look at the logs manually, you head over to the Azure portal.

1. Go to **Microsoft Entra ID**.
    
2. Look in the **Monitoring Group**.
    
3. Click **Sign-in Logs**.
    

Once you open that blade, it gives you a clean list with the essentials: the **Date** it happened, the **User** involved, the **Application** they tried to access, whether the **Status** was a success or failure, and most importantly, the **Conditional Access** policy that decided whether to let them in or block them.

### What is coming up next

In the next section, we are learning about **Microsoft Secure Score**. Now that you know how to hunt for threats and track sign-ins, we are going to learn how Microsoft grades your security posture and tells you exactly which "doors" you need to lock to get a higher score and stay safe.