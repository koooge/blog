---
title: "Passed after failing AWS certification SAP"
date: 2021-07-05T21:06:57+02:00
categories: ["aws"]
translationKey: "aws_sap"
draft: false
---

I took the SAP (Solutions Architect Professional) exam because my AWS SAA certification was almost expired. As online is not uncommon nowadays, I took the exam at PSI-Online which is possible to take it from home.

Although I passed the exam, I was disqualified once due to an inadequate environment, and then passed after repeated inquiries. I would like to write about my struggle in the hope that it will be helpful to others.

## Took the exam online
It was a good way to take the exam online in this pandemic. There are two options PSI-Online and Pearson VUE. I chose PSI-Online because Pearson VUE did not have variety of schedule enough.

### Check the software beforehand
To take the exam at PSI-Online, you need to install a special software.

About 30 minutes before the exam, I installed the software on Windows and when I tried to connect to the exam, Hyper-V and other background processes were running and I could not start the exam. I switched to a mac in a hurry and took the exam, but I missed the start time by about 10 minutes.

It was the first time I took the exam, and I was in a great hurry. I think it's better to install and check the startup as soon as possible in order to take the exam calmly.

### Forced disconnection, disqualification, and SAA revocation
Four minutes before the end of the exam, I had finished solving all the questions and was reviewing them. That's when the incident happened.

Suddenly the words **VIOLATION** appeared on the screen, and the software force-quit, as if I had pressed ALT+F4. I couldn't figure out what was going on. So I tried to reconnect, but the screen said the exam was over and I couldn't connect.

I contacted AWS support it since I had no choice thad day. A couple of the days later, I received the following email from Credly, which issues badges for AWS certification.

![saa](saa.jpg)

*(Japanese) The Badge has been revoked. (SAA)*

AWS support also responded.

![aws-ans](aws-ans.jpg)

*(Japanese) The proctor confirmed that another program was running in the background on your PC during your exam. Since this was a violation of policy, the exam was suspended by the proctor. You will be asked to pay the examination fee again and retake the examination.*

I was disqualified from the SAP exam since I was considered in violation. In addition, my certified **SAA certification was revoked**.

Furthermore, my AWS training account was also suspended.

![aws](aws.jpg)

*(Japanese) Your account has been suspended and you will not be able to access the system. Please contact our customer support for assistance.*

Are you kidding?

### Contact to support
I was not satisfied, so I contacted support, but it was a long process(1 month) to resolve. The inquiry went back and forth from AWS -> PSI -> AWS -> PSI -> and so on. Each company has their own issue tracker, and we had to explain the same thing to each one.

PSI almost always came back with nothing but **ask AWS** templates.

```

Please click the link below to contact AWS directly.

AWS Link: https://aws.amazon.com/contact-us/?nc1=h_ls

```

After all, it seems that PSI-Online is only doing the test on behalf of the company, and there is nothing they can do about the results they have produced.
I continued to patiently ask AWS support for confirmation.

I told them that I could not accept the deprivation of the SAA because I had not committed any fraud.

![aws-ans2](aws-ans2.jpg)

*(Japanese) The answer is after escalation to the certification team.*

Oh, I see..

### Suddenly passing SAP
I kept communicating with AWS support, and suddenly I received a SAP acceptance notice. I was also able to log in to AWS training and was treated as recertified for SAA.

![aws-fin](aws-fin.jpg)

*(Japanese) Our certified security team reviewed the exams for the opinions that our customers have taken. Since you have reached the passing score at the end of the exam, the result of this exam is "Pass".*

First of all, I was surprised because I had thought it would be a blessing if I could confirm that I had not cheated and withdraw my SAA revocation. Rather than being happy to have passed the exam, I was happy to finally be free from the hassle. I've never seen such unhappy qualification notice.

*(Japanese) If you take the test in the next time, please note that opening another application or program during the test will invalidate your test results.*

Anyway, I felt the AWS certified support was a bit arrogant. AWS doesn't need to worry about it. I'll never take another AWS certification exam again.I'm tired.

## Extra: How to learn SAP
As an appendix, I share the following to study materials I used. E-learning in particular is a must.

I focused on migrating from on-premise environment to AWS, and on-premise and AWS hybrid architecure. Because I didn't have experience of the on-premise and AWS hybrid architecture enough.

- Official E-learning: https://www.aws.training/Details/eLearning?id=42403
- Problem collection (paid)(Ja): https://www.udemy.com/course/aws-53225/
- Official practice exams (Free coupons were given out when I passed the SAA. The exam was also half price with the coupon)

I recommend taking the exam in your native language. All of the questions are difficult to answer correctly. But it would be better to check the English as well because there are rather many mistranslations. For example, the service name was wrong (CloudFront in Japanese, CloudFormation in English)
