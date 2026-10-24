At a micro level, from a single application perspective, a monolith somehow promotes agility because it tends to ignore whatever exists in the enterprise landscape. A team developing a monolith does not care at all about what is around; they just do what they need. They do not hesitate to make point-to-point connections with other applications or databases if needed, to achieve project-specific goals. This makes the team gain speed and autonomy, with the risk of becoming the big monolith shown in Figure 5.1, should the application grow over time.

At a macro level, multiplying smaller monoliths may result in an enterprise-level monolith, which is way more problematic. If the whole enterprise landscape has become a plate of spaghetti, no one will ever dare to change any of the micro applications anymore, because of an increased risk of cascading side effects. So, as you have probably guessed already, monoliths should be avoided as much as possible. Nevertheless, let’s see what benefits they offer.

# Benefits of monoliths

It might be hard to believe but monoliths also have benefits. Here are a few of them:

+   - Developing a monolith is easy. You do not need to learn any software architecture methodology. Their simplicity is probably why we have so many of them.
+   - They are simple to deploy. Since they are, most of the time, packaged as a single package, redeploying the application is just about deploying a single package.
+   - They are simple to monitor. Again, monolithic applications are usually deployed on a single machine with a database server next to it. In some cases, you might have multiple frontend/application servers, but there are not too many places to look when you're troubleshooting a problem.

The keyword is *simplicity*, at least as long as the application remains lightweight. Now, let’s look at some of the drawbacks.

# Challenges of monoliths

Admittedly, monoliths have more drawbacks than advantages:

+   - Adding new features and fixing bugs becomes complicated. As stated in the previous section, most of the monoliths are packaged as a single package. While this can be considered an easy way to deploy the application, every single feature or bug fix will cause you to redeploy the entire application, which will cause a temporary (hopefully) outage and require many regression tests. This harms *testability*, and ultimately the *deployability* quality attribute. Over time, the team will lack agility, which impacts *upgradability*.