---
title: "Guide to IP and Domain Warming and Migrating to Amazon SES"
date: 2025-07-03
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Guide to IP and Domain Warming and Migrating to Amazon SES

**By Tyler Holmes | 03 JUL 2025**

**Categories:** Amazon Simple Email Service (SES), Messaging

---

## Introduction

Transitioning your email workloads from another email service provider (ESP) to [Amazon Simple Email Service (Amazon SES)](https://aws.amazon.com/ses/) can be a challenge, given that each workload can be unique. In this post, we show you how to successfully warm up IP addresses and domains when migrating to Amazon SES. This guide aims to provide a comprehensive overview of IP and domain warming best practices so you can make your transition to Amazon SES as smooth as possible. We discuss some of the challenges you might encounter and how to overcome those common pitfalls when transitioning to a new email service provider (ESP).

---

## Understanding IP and Domain Email Warming

IP warming and domain warming are strategic processes designed to gradually introduce a new sending identity to mailbox providers. A new sending identity can be a dedicated IP address, new domain, a subdomain of that domain, or any combination of them. The core objective of warming is to build a positive reputation with mailbox providers so your emails are delivered to the inbox rather than being filtered into spam folders or potentially blocked from being delivered to a mailbox altogether.

Mailbox providers such as Gmail, Yahoo, and Outlook are vigilant about protecting their users from spam and malicious content. When you introduce a new sending identity, mailbox providers evaluate the new sending identity with caution. They evaluate the early sending from the domain and IPs to ensure they're sending the mailbox provider's users messages that are wanted and aren't engaged in abusive practices such as spam or phishing. Warming provides mailbox providers the opportunity to observe your sending patterns, content, and engagement metrics, allowing them to gradually build trust in your new sending identity.

Warming can be different for each scenario. For example, you can have completely warmed IPs, but if your sending domain is new, you'll likely have to warm it up as well but you won't need to worry about IP warming as much. Another common scenario is that of adding a new IP but sending with an established domain. In this case, the IP will need warming, but the domain itself is helping the warming because it already has an established reputation. When you have a net new IP and a net new domain, you'll have to warm them together. The warm-up best practices we outline in this post, such as starting out slow and targeting your highest engaged subscribers first, apply to your situation.

---

## Why Warming is Critical

Warming is essential for several reasons, each contributing to the overall success and reliability of your email marketing campaigns:

### 1. Building Trust with Mailbox Providers

A positive sender reputation is crucial for email deliverability. Mailbox providers use complex algorithms to evaluate the reputation of senders, and warming helps them build trust in your new sending identity.

### 2. Avoiding Initial Deliverability Issues

When you switch to a new ESP or introduce a new sending identity, it's common to experience an initial dip in deliverability metrics, such as lower open rates, click-through rates, or higher bounce rates. Warming can mitigate these issues by giving mailbox providers time to adapt to your new sending patterns, whether that is a new domain, subdomain, or new IP infrastructure.

### 3. Maintaining Consistent Sending Behavior

Warming encourages you to maintain a steady and predictable sending cadence. Sudden, significant changes in sending volume, content, or frequency can trigger inbox spam filters because such changes might indicate that the sender has been compromised or is engaging in abusive practices. Even anomalies such as large changes in volume or throughput during seasonal events such as Black Friday can be interpreted as negative, and mailbox providers take a cautious approach when they detect anomalies such as sudden large spikes in volume.

### 4. Long-term Deliverability Success

It's a misconception that warming is done only one time. In reality you need to maintain traffic volumes and sending cadences to keep those sending identities warm. Additionally, if you plan on increasing volume considerably, for example from 1M to 5M or 5M to 25M, you need to warm up to those volumes. Those large jumps in volumes look suspicious to inbox providers even if you've been sending consistently.

### 5. Adapting to Mailbox Provider Changes

Mailbox providers also continuously update their algorithms to better detect spam and abusive behavior. If you view warming as a constant process and consistently monitor your deliverability and engagement signals you can make adjustments to your sending strategy as needed, ensuring that your emails continue to reach the inbox, even as inbox and audience behavior changes.

---

## Common Challenges to Moving Traffic and Warming Up on a New ESP

Transitioning your email traffic to a new ESP can present unique challenges that require careful consideration and strategic planning to overcome. These challenges include the following:

1. **Event-driven traffic** – If all your email is event-driven, it's hard to control volume and throughput.

2. **Multiple sending domains** – Having many sending domains with varying traffic volumes and throughputs can complicate the transition.

3. **No shared IPs** – Some organizations aren't allowed to use shared pools of IPs.

4. **Lack of engagement data** – Absence of data related to engagement can make it difficult to optimize the warming process.

5. **Outdated bounce and unsubscribe info** – Not having up-to-date bounce and unsubscribe information in your current ESP can lead to deliverability issues.

6. **Single second-level domain** – You're currently sending your mail from your second-level domain, such as `example.com`, without separate subdomains for logical use cases such as transactional or marketing.

7. **Tight timelines** – Contracts ending or other reasons might impose a tight timeline for the transition.

8. **Challenges for independent service providers (ISVs) and software-as-a-service (SaaS) providers** – These organizations often don't have complete control over the volume, content, lists, or sending consistency of their customers. They also might not have direct access to the DNS needed to update and align sending domains and authentication.

---

## Strategies for a Successful Warm-up and Migration

The following list isn't suitable for every case, and many customers will use more than one strategy to address their challenges and smooth their transition to Amazon SES:

### 1. Send to Your Best Audience First

The most important thing you need to do when transitioning to a new ESP is to send to your highest and most active recipients on the new ESP and leave the less active or risky segments on the previous ESP until you're ready to full switch. For example, if you're a daily sender who sends to 1M addresses a day and have an open rate of about 20%, you need to start onboarding with segments that include those who are opening. A good strategy is to start with openers from the last 30 days, then move to openers from 31–90 days, and so on.

### 2. Gradually Move Your Less Engaged Subscribers

After you've transitioned your most active segments, you can start to include the less engaged a little at a time. You can sprinkle them in with the more engaged segments so that if you do get bounces or complaints it is buffered by the more engaged segments. Make sure to continue monitoring for issues and immediately stop increasing your workloads if you encounter deliverability issues such as increased bounce or spam rates.

### 3. Start with Predictable Workloads

Begin with workloads that aren't time-dependent, such as newsletters, which are easier to control and monitor.

### 4. Batch Event-driven Messages

For event-driven messages that aren't time-sensitive, try to batch and spread them out to manage the volume.

### 5. Use Automated Warm-up Processes

[Standard](https://docs.aws.amazon.com/ses/latest/dg/manual-dedicated-ips.html) and [managed dedicated IPs](https://docs.aws.amazon.com/ses/latest/dg/managed-dedicated-sending.html) can help to manage the daily volume by allowing predefined levels of traffic on your dedicated IPs and spilling over into shared IP pools when the volume of email has reached a level we deem to be sufficient for your dedicated IPs. This is dependent on your warm-up progress thus far. Dedicated standard is a static 45-day increase, but managed dedicated has a more sophisticated process. To learn more, refer to [Dedicated IP addresses for Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html).

### 6. Strategically Use Shared IP Pools

Use shared IP pools for workloads that don't require dedicated IPs. Because there is consistent volume already going through these IPs, they're a little more forgiving than dedicated IPs being warmed up.

### 7. Transition Gradually to Dedicated IPs

Begin with shared IPs and gradually transition to dedicated IPs as they warm up.

### 8. Transition Gradually to Logical Subdomains

Split your traffic into logical workloads that can have consistent volume and throughput. Even something as simple as `marketing.example.com` and `transactional.example.com` is better than sending mail from `example.com`

### 9. Onboard New Customers on the New ESP

For ISVs and SaaS providers, consider onboarding new customers directly on the new ESP to gather initial data and test the waters. New customers already need to be warmed up, so if you warm them up on Amazon SES rather than your legacy ESP, you don't need to go through a warming process twice.

---

## Prepare to Migrate Email Traffic to Amazon SES

Before you migrate your email program to Amazon SES, it's important to thoroughly document and organize your existing setup. This preparatory work will lay the foundation for a successful warm-up and migration process.

### Preparation Checklist

1. **Document your use cases** – Categorize your use cases as either marketing or transactional. This will help you understand the nature of the emails you send and how they should be handled.

2. **Document your sending domains** – Include the "from" names associated with each domain. This will assist in mapping the appropriate domain to the corresponding email type. Ideally, you should avoid sending from your root domain. For example, use a subdomain such as `email.brand.com` instead of `brand.com`. Review and document your authentication (for example, SPF, DKIM, or DMARC). In some cases, you might not need to align all of them, but you'll definitely need to [align DMARC as part of the bulk sender requirements](https://aws.amazon.com/blogs/messaging-and-targeting/an-overview-of-bulk-sender-changes-at-yahoo-gmail/).

3. **Map use cases to sending domains and from names** – Create a clear correspondence to ensure the right emails are sent from the appropriate domains. At a minimum, it's a best practice to have separate subdomains for transactional and promotional email use cases, such as `transactional.brand.com` and `promo.brand.com`.

4. **Document volume and max throughput** – Capture this information for each use case mapped to your sending domains. This will help you understand the scale of your email operations and plan your architecture and warming strategy accordingly.

5. **Anticipate a temporary dip in deliverability metrics** – While transitioning to a new ESP, you might experience a short-term fluctuation in metrics such as open rates and click-through rates. This is a common occurrence and shouldn't be viewed as a failure of the service. It's an expected part of the migration process as mailbox providers adapt to your new sending identity. By closely monitoring your bounce and complaint rates, you can make proactive adjustments to your ramp-up plan to ensure a smooth transition.

6. **Document your warming plan** – Have a plan to gradually increase traffic for each identity and monitor engagement metrics. Plan for how to address high bounce or complaint rates.

### Sample Warm-up Plan

The following table shows a sample warm-up plan. Notice that the days are categorized by large inbox providers. This is because these providers all accept new mail at different rates. Categorizing this way is a recommended best practice, but if you can't segment that granularly, then you can use the Daily totals column as a guide. The AWS managed dedicated IP service automatically does this segmentation and throttling at the domain level for you.

The following plan is a typical ramp. You can get more aggressive the higher your overall engagement rates are, so if you're at 40–60% engagement, you can use this warm-up. If your rates are lower, you might want to be a little more conservative. Make sure to be adaptive as you go into your warming plan because you might need to maintain the same rate for a couple days or even roll back a step if you're experiencing negative trends such as a drop in deliverability or engagement. Remember, as you get into the less engaged segments of your list, engagement will drop, but it shouldn't be drastic. Constantly monitor your metrics during this critical time.

| Day | @gmail.com | @hotmail.com | @outlook.com | @yahoo.com | @icloud.com | @aol.com | Others | Daily Total |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 150 | 150 | 150 | 150 | 150 | 150 | 150 | 1,050 |
| 2 | 300 | 300 | 300 | 300 | 300 | 300 | 300 | 2,100 |
| 3 | 600 | 600 | 600 | 600 | 600 | 600 | 600 | 4,200 |
| 4 | 1,200 | 1,200 | 1,200 | 1,200 | 1,200 | 1,200 | 1,200 | 8,400 |
| 5 | 2,400 | 2,400 | 2,400 | 2,400 | 2,400 | 2,400 | 2,400 | 16,800 |
| 6 | 5,000 | 5,000 | 5,000 | 5,000 | 5,000 | 5,000 | 5,000 | 35,000 |
| 7 | 10,000 | 10,000 | 10,000 | 10,000 | 10,000 | 10,000 | 10,000 | 70,000 |
| 8 | 20,000 | 20,000 | 20,000 | 20,000 | 20,000 | 20,000 | 20,000 | 140,000 |
| 9 | 40,000 | 40,000 | 40,000 | 40,000 | 40,000 | 40,000 | 40,000 | 280,000 |
| 10 | 80,000 | 80,000 | 80,000 | 80,000 | 80,000 | 80,000 | 80,000 | 560,000 |
| 11 | 150,000 | 150,000 | 150,000 | 150,000 | 150,000 | 150,000 | 150,000 | 1,050,000 |
| 12 | 300,000 | 300,000 | 300,000 | 300,000 | 300,000 | 300,000 | 300,000 | 2,100,000 |
| 13 | 425,000 | 425,000 | 425,000 | 425,000 | 425,000 | 425,000 | 425,000 | 2,975,000 |
| 14 | 500,000 | 500,000 | 500,000 | 500,000 | 500,000 | 500,000 | 500,000 | 3,500,000 |
| 15 | 600,000 | 600,000 | 600,000 | 600,000 | 600,000 | 600,000 | 600,000 | 4,200,000 |
| 16 | 650,000 | 650,000 | 650,000 | 650,000 | 650,000 | 650,000 | 650,000 | 4,550,000 |
| 17 | 700,000 | 700,000 | 700,000 | 700,000 | 700,000 | 700,000 | 700,000 | 4,900,000 |
| 18 | 800,000 | 800,000 | 800,000 | 800,000 | 800,000 | 800,000 | 800,000 | 5,600,000 |
| 19 | 900,000 | 900,000 | 900,000 | 900,000 | 900,000 | 900,000 | 900,000 | 6,300,000 |
| 20 | 1,000,000 | 1,000,000 | 1,000,000 | 1,000,000 | 1,000,000 | 1,000,000 | 1,000,000 | 7,000,000 |
| 21 | 1,100,000 | 1,100,000 | 1,100,000 | 1,100,000 | 1,100,000 | 1,100,000 | 1,100,000 | 7,700,000 |
| 22 | 1,200,000 | 1,200,000 | 1,200,000 | 1,200,000 | 1,200,000 | 1,200,000 | 1,200,000 | 8,400,000 |
| 23 | 1,300,000 | 1,300,000 | 1,300,000 | 1,300,000 | 1,300,000 | 1,300,000 | 1,300,000 | 9,100,000 |
| 24 | 1,400,000 | 1,400,000 | 1,400,000 | 1,400,000 | 1,400,000 | 1,400,000 | 1,400,000 | 9,800,000 |
| 25 | 1,500,000 | 1,500,000 | 1,500,000 | 1,500,000 | 1,500,000 | 1,500,000 | 1,500,000 | 10,500,000 |
| 26 | 1,600,000 | 1,600,000 | 1,600,000 | 1,600,000 | 1,600,000 | 1,600,000 | 1,600,000 | 11,200,000 |
| 27 | 1,700,000 | 1,700,000 | 1,700,000 | 1,700,000 | 1,700,000 | 1,700,000 | 1,700,000 | 11,900,000 |
| 28 | 1,800,000 | 1,800,000 | 1,800,000 | 1,800,000 | 1,800,000 | 1,800,000 | 1,800,000 | 12,600,000 |
| 29 | 1,900,000 | 1,900,000 | 1,900,000 | 1,900,000 | 1,900,000 | 1,900,000 | 1,900,000 | 13,300,000 |
| 30 | 2,000,000 | 2,000,000 | 2,000,000 | 2,000,000 | 2,000,000 | 2,000,000 | 2,000,000 | 14,000,000 |
| 31 | 2,100,000 | 2,100,000 | 2,100,000 | 2,100,000 | 2,100,000 | 2,100,000 | 2,100,000 | 14,700,000 |
| 32 | 2,200,000 | 2,200,000 | 2,200,000 | 2,200,000 | 2,200,000 | 2,200,000 | 2,200,000 | 15,400,000 |
| 33 | 2,300,000 | 2,300,000 | 2,300,000 | 2,300,000 | 2,300,000 | 2,300,000 | 2,300,000 | 16,100,000 |
| 34 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 16,800,000 |
| 35 | 2,500,000 | 2,500,000 | 2,500,000 | 2,500,000 | 2,500,000 | 2,500,000 | 2,500,000 | 17,500,000 |
| 36 | 2,600,000 | 2,600,000 | 2,600,000 | 2,600,000 | 2,600,000 | 2,600,000 | 2,600,000 | 18,200,000 |
| 37 | 2,700,000 | 2,700,000 | 2,700,000 | 2,700,000 | 2,700,000 | 2,700,000 | 2,700,000 | 18,900,000 |
| 38 | 2,800,000 | 2,800,000 | 2,800,000 | 2,800,000 | 2,800,000 | 2,800,000 | 2,800,000 | 19,600,000 |
| 39 | 2,900,000 | 2,900,000 | 2,900,000 | 2,900,000 | 2,900,000 | 2,900,000 | 2,900,000 | 20,300,000 |
| 40 | 3,000,000 | 3,000,000 | 3,000,000 | 3,000,000 | 3,000,000 | 3,000,000 | 3,000,000 | 21,000,000 |

---

## Best Practices for a Successful IP Warm-up

A successful IP warm-up involves a strategic approach that combines technical preparation, engaged subscribers, compelling content, and ongoing monitoring.

### 1. Ensure Technical Readiness

[Configure DNS records](https://docs.aws.amazon.com/ses/latest/dg/configure-identities.html) and set up [SPF](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-spf.html), [DKIM](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim.html), [DMARC](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dmarc.html), and [BIMI](https://aws.amazon.com/blogs/messaging-and-targeting/what-is-bimi-and-how-to-use-it-with-amazon-ses/) so your email content complies with best practices. Make sure your [DMARC is aligned](https://aws.amazon.com/blogs/messaging-and-targeting/navigate-bulk-sender-requirements-with-amazon-ses/) if you're sending across multiple ESPs or applications.

### 2. Use an Engaged, Permission-based Mailing List

Use a clean, opt-in list of subscribers who are interested in your content. For more information, refer to [Optimizing Email Deliverability: A User-Centric Approach to List Management and Monitoring](https://aws.amazon.com/blogs/messaging-and-targeting/optimizing-email-deliverability-a-user-centric-approach-to-list-management-and-monitoring/).

### 3. Provide Compelling, Valuable Email Content

Send content that resonates with your audience and encourages engagement.

### 4. Gradually Ramp Up Sending Volume and Cadence

Start with a small volume of emails and gradually increase over time to allow mailbox providers to observe your sending patterns.

### 5. Maintain Consistency in Sending Behavior

Avoid sudden, significant changes in sending volume, content, or frequency.

### 6. Continuously Monitor and Optimize Key Metrics

Track open rates, click-through rates, bounce rates, and complaint rates, and make adjustments as needed. For more information, refer to [Amazon SES – Set up notifications for bounces and complaints](https://aws.amazon.com/blogs/messaging-and-targeting/amazon-ses-set-up-notifications-for-bounces-and-complaints/).

### 7. Ongoing Maintenance of Sender Reputation

Audit data flows, ramp up changes gradually, and follow evolving email marketing best practices.

---

## Navigating Initial Deliverability Challenges

When transitioning to a new ESP, you might encounter some initial deliverability challenges. It's important to monitor and exercise caution if you observe increased bounce or complaint rates. If you do have challenges, you need to address them promptly. Maintain the same volume or even reduce the volume the next day if you encounter these issues:

### 1. Spike in Hard Bounce Rates

Bring over your suppression lists from the ESP you're offboarding from, but if your previous ESP didn't manage these well or you load some old addresses you weren't aware of, it's common to experience hard bounce spikes at the beginning. If this happens, slow your volume increases or even stop increasing until things stabilize. It's more important to warm up properly than it is to get to production levels of sending as fast as possible. This is one more reason that it's always best to start with your most engaged segments.

### 2. Increased Spam Complaints

Emails might reach recipients who previously filtered them, leading to more spam complaints. Changing an identity can also cause your recipients to hit the spam button because they don't recognize it. Announce identity changes before changing your ESP to reduce the chances of an issue.

### 3. Heightened Mailbox Provider Scrutiny

Mailbox providers will closely monitor new senders to confirm they're not engaged in malicious activities. This can divert emails to the spam filter initially or even be throttled if you reach volume or throughput limits. Gmail is known to be stringent. Amazon SES managed dedicated IPs use our data to know how much mail the big inbox providers will accept while you're warming up and keep you from overshooting their limits.

### 4. ESP Throttling and Sending Limits

The new ESP might have stricter rules regarding the volume of emails that can be sent to individual mailbox providers. Amazon SES has account limits for daily volume and max throughput, so adjust yours to what you'll need. To learn more, refer to [Increasing your Amazon SES sending quotas](https://docs.aws.amazon.com/ses/latest/dg/manage-sending-quotas-request-increase.html) in the Amazon SES Developer Guide.

---

## Maintaining IP Reputation After Warming

IP warming is an ongoing process. Even after the initial warm-up phase, it's essential to maintain your sender reputation by continuously managing your email program. Your subscriber engagement might fluctuate as your list grows and changes. Similarly, ramping up email volume for a seasonal campaign will require adjustments to your warm-up process. You need to be proactive and adapt your IP warming strategy.

Audit data flows and campaigns and monitor email list sources, data collection practices, and campaign performance. When introducing new elements, do so incrementally to avoid triggering reputation issues. To allow time for your reputation to stabilize, provide at least a month for a new baseline to be established after major program changes. Engage with customers, provide value, and implement re-engagement campaigns to nurture your customer relationships. Adhere to evolving email marketing best practices, including proper authentication protocols and emerging technologies. Be proactive and track domain and IP reputation so you can quickly address the deliverability issues that arise. To learn more about monitoring inbox tools such as Google Postmaster, refer to [Understanding Google Postmaster Tools (spam complaints) for Amazon SES email senders](https://aws.amazon.com/blogs/messaging-and-targeting/understanding-google-postmaster-tools-spam-complaints-for-amazon-ses-email-senders/).

---

## Conclusion

Transitioning your email program to a new ESP such as Amazon SES can seem complex, but it can be quick and seamless if you follow the best practices explained in this post. IP warming is a critical component of this process because it helps build a positive sender reputation with mailbox providers and promotes the reliable delivery of your emails.

Throughout this guide, we've covered the key aspects of IP warming and email migration, from understanding the importance of this practice to identifying common challenges and outlining effective strategies for a successful transition. By following best practices such as facilitating technical readiness, using an engaged subscriber base, providing compelling content, and gradually ramping up sending volume, you can navigate the initial deliverability challenges and establish a strong foundation for long-term email program success.

However, the work doesn't stop when the initial warm-up phase is complete. Maintaining IP reputation and adapting your strategy as your email program and subscriber engagement evolve is an ongoing process. Continuously monitoring key metrics, auditing data flows, and staying up to date with evolving email marketing best practices is crucial for sustaining deliverability. A long-lasting sender reputation and enduring relationships with your list and recipients are some of the key benefits of following these best practices. Transitioning to a new ESP is a significant undertaking, but with the right preparation, execution, and commitment to ongoing maintenance, your migration can be smooth and successful.

---

## About the Author

### Tyler Holmes

![Tyler Holmes](https://d2908q01vomqb2.cloudfront.net/632667547e7cd3e0466547863e1207a8c0c0c549/2025/07/02/image-1-2.png)

Tyler is a Senior Specialist Solutions Architect. He has a wealth of experience in the communications space as a consultant, an SA, a practitioner, and leader at all levels from Startup to Fortune 500. He has spent over 14 years in sales, marketing, and service operations, working for agencies, consulting firms, and brands, building teams and increasing revenue.

---

## Resources for Deliverability

- [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html)
- [Amazon SES API documentation](https://docs.aws.amazon.com/ses/latest/APIReference-V2/Welcome.html)
- [Service quotas in Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/quotas.html)
- [Amazon SES endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ses.html#ses_region)
- [Amazon SES pricing](https://aws.amazon.com/ses/pricing/)
- [Amazon Pinpoint](https://aws.amazon.com/pinpoint/product-details)
- [Getting Started with Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/gettingstarted.html)

---

**Tags:** Amazon SES, Email Deliverability, IP Warming, Domain Warming, Email Migration, Email Marketing
