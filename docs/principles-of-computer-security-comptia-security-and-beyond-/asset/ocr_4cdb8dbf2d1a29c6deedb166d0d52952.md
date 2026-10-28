those companies for surveys, newsletters, and more.

A hard fail, represented by -all (where the tilde is replaced with a dash), will cause the e-mail to be rejected and dropped on the spot. Banks might use the hard fail option, since phishing attacks using spoofing can cause great damage. The logic is that there shouldn’t be many IP addresses that can send e-mail on behalf of the bank. The hard fail can lead to false positives, though. SPF has had problems with mail forwarding services and mailing lists, which could lead to e-mails getting dropped with a hard fail set.

SPF doesn’t actually instruct receiving mail servers what to do with e-mails that don’t pass SPF checks. While the sending domain can specify -all to cause a hard fail, in reality, this hasn’t been followed by all receiving mail servers. Interestingly enough, the From: field in the e-mail is not validated with SPF but rather the Return-Path value, which is the address where any bounce messages about delivery problems should be sent.

The message can be logged and either dropped or moved to Spam.

Later on in this lab exercise, we’ll see that DMARC steps into the game now and helps receiving mail servers determine the next course of action for an e-mail that failed SPF.

Companies use SPF as one factor, included with others, to determine what should happen to that e-mail if it fails SPF.

Here’s one example from https://hi.service-now.com/kb_view.do?sysparm_article=KB0755173:

Your ServiceNow email relay system uses SpamAssassin to determine if an email is a spam by assigning points based on several spam tests.

SPF checks factor heavily into a mail’s spam score. In order to be flagged as spam, a message must have an aggregate score of 6.2 or higher. A soft SPF failure will add 3.5 to the score, whereas a hard SPF failure will add 7.0 to the score, immediately flagging it as spam. It is recommended to check and ensure that your company’s SPF records are correct and up-to-date, or some messages may be inadvertently marked as spam.