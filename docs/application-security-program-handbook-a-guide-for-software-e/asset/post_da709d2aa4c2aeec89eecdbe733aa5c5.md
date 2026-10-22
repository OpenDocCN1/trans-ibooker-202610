
On a daily basis the application security team members perform these tasks as their core function, but there is other ad hoc work that happens such as general consulting on design and strategy across the organization. It is often hard to find the “unicorn” application security person that can do all or even multiple items from the core functions. This requires the application security team to hire multiple people with more focused skill in the core areas. For instance, in order to meet the needs of the functions the team may hire the following roles:

![](img/cc84747f93d290964598894b4f5c153b_126_0.png)

> NOTE This is a very unpopular opinion, but one that a lot of my application security peers share: Penetration testers are not application security engineers. Many organizations work hard to build a team that can perform penetration tests against applications. Don’t get me wrong, this is needed. However, this is not the primary function of a mature application security team.

The organization will also have to sprinkle in the obligatory support around the team like leadership and project management. As you can see, this team can focus on delivering security to the organization through the tools, processes, and people. However, scaling the team to meet the organization’s needs is next to impossible. The recommended size of the application security team varies and has been proposed as 10:1 of software engineer to application security engineer. In large organizations, this ratio is difficult to meet and depends on how seriously it takes building security into their applications. However, in most cases, the application security team is roughly 1% of the development headcount. Again, this is not a recommendation, but rather what is typically seen in the industry. I have been a part of teams that are oversized and undersized based on that measurement. This is why creating a culture that supports the security of the applications that are being developed is critical to reducing the overall risk of organization.

> EXERCISE Take a look at your own organization’s application security team and understand what functions they perform. If your organization doesn’t have one, research some popular organizations and their application security team structure. Each organization I have worked for has had a different set of goals and staffing model to support application security. You will never find the same model in every organization.

## 5.1.2 Just hire more application security people

In most engineering organizations, when you have a new project or push to meet a deadline, the organization will just hire more engineers, testers, architects, and others to meet the need. This is not only impractical for application security but is also often cost-prohibitive. Security teams in an organization are often working on a smaller budget compared with the overall IT spend. Although the spend varies by organization and industry—and there was a significant increase in security spend during the global pandemic in 2020—most organizations spend 5-15% of their IT budget on security. The bulk of this spend goes to security operations and perimeter defense tools like a security information and event management (SIEM), network detection and protection, firewalls, and others. The application security team typically gets a small portion of that budget that is mostly dedicated to personnel.

Despite spending most of their budget on personnel the application security team can never scale to the size needed. However, this is not the biggest problem facing the application security team. It’s actually very difficult to hire the people you need. There has been a term that has been used a lot in the industry called “negative unemployment.” This means that there are more jobs open than qualified people to fill them. To be clear, this is not just application security, but across the board in security like analyst, network security, information security specialist and others. This means there are millions of jobs left unfilled in security. Those of you reading this book that work in security know exactly what I’m talking about.

The reality is that application security resources are even more difficult to locate than other personnel in their peer groups. Generally, you are looking for someone that has not only security expertise but also development experience. Your best bet is to find someone that has been a developer and gained an interest in security. The importance of finding someone that has development experience cannot be overstated. Someone with this background brings the following to the team when they join:

- Understands the development pipeline
- Can interpret code and perform code reviews
- Understands the overall development process
- Can speak the language of the development team

Having these qualities as well as an understanding of security will be an asset to any application security team. This allows them to take vulnerabilities that impact an application and be able to interpret this issue in a way that is understood by the development team. For instance, a prototype pollution issue may have been discovered through a DAST tool and assigned to be triaged by the application security team. These issues can be complex to describe and walk through with the engineering team if the application security member cannot describe how prototypes works in JavaScript. Having a background in development means that the application security person can at least describe the issue, how it impacts the application and help develop mitigations. More importantly, when the development team makes the claim that the vulnerability does not impact them, which they will do, then an application security person who knows development will be able to determine whether the impact is real.

You can see how these types of hires can be hard to find. With a constrained budget, and a small pool of resources to hire from, the application security team will struggle to find the right people that can deliver security to a development organization.

Organizations can also make it difficult on themselves when they don’t have a good job description or even job title for what they are looking for. An application security architect is different than a solution architect. A security engineer is different than a security analyst. Even worse, some organizations will have their own internal titles that may map to industry standard job titles, but only muddy the waters. This means that well-qualified individuals may pass on a role due to a misunderstanding of the title.

### Definition

Each organization has a different description for common roles. Here are a few of the common application security roles and a brief description:

- Application Security Architect – Represent the application security team on strategy in the organization and develop secure architecture, standards, and documentation.
- Application Security Engineer – Work closely with the application development team to ensure that security is integrated in the development process for the development team.
- DevSecOps Engineer – Take the responsibility of the application security engineer with the added responsibility of automating the processes in the development pipeline.
- Penetration Tester – Provide testing of applications for security related issues and develop summary reports on findings.

One last point I will make regarding hiring in application security. Sometimes the right person for the team has no clue about security. The application security team is usually operating parallel to the engineering teams they work with, which means that having members of the team that understand the engineering process is as important and, depending on the role, sometimes more important than understanding security. The team will need people that can integrate tools, develop and understand code, speak intelligently about the software development process, and operate and tune tools. To achieve this, the team does not always need to hire the best and brightest security people as these gaps need to be filled with a team that is diverse in thought and background.

## 5.1.3 How to close the gap

So, what does this mean for an organization that wants to raise their security posture without having to hire a massive army of penetration testers, security engineers, and security architects? When you can’t afford a car, but you need to drive to the store, find a friend that has one and borrow theirs.

> 'Look for the helpers. You will always find people who are helping.'
> 
> Fred Rogers

Getting anything done in application security usually means that you will be borrowing time from the resources in engineering or at least working with the product and engineering teams to jostle for space in their releases. In most cases you will want to make this as transparent and open as possible. Nothing drives a manager crazy, me included, more than their team getting pulled into tasks that are unrelated to the work that they should be doing. However, security is everyone’s responsibility, right? It’s also the responsibility of the
