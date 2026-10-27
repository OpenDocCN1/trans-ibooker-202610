(continued)
If you’re entirely new to AWS, we highly recommend you pick up a copy of Michael and Andreas Wittig’s book, *Amazon Web Services in Action* (Manning, 2018). The first chapter in this book includes a well-written tutorial at the end of the chapter on how to sign up and configure your AWS account. That chapter is available for download here:

https://www.manning.com/books/amazon-web-services-in-action-second-edition

In this chapter, we’ve tried as much as possible to use the free-tier services offered by Amazon. The only place where we couldn’t do this was when setting up the EKS cluster. We used an M4 large server that costs approximately $0.10 per hour to run. If you don’t want to incur high costs running this server, ensure that you shut down your services after you’re done. If you want to know more about AWS EC2 instance pricing, visit the official Amazon documentation at the following link: https://aws.amazon.com/ec2/pricing/on-demand/.

And finally, there’s no guarantee that the Amazon resources (Postgres, Redis, and EKS) that we use in this chapter will be available if you want to run this code yourself. If you’re going to run the code from this chapter, you need to set up your own GitHub repository (for your application configuration), your own Jenkins environment, Docker Hub (for your Docker images), and Amazon account. You’ll then need to modify your application configuration to point to your account and credentials.

# 12.2.1 Creating the PostgreSQL database using Amazon RDS

To begin, we need to set up and configure our Amazon AWS account. Once this is done, we’ll create the PostgreSQL database that we’ll use for our O-stock services. To do this, we first need to log in to the Amazon AWS Management Console (https://aws.amazon.com/console/). You’ll be presented with a list of Amazon web services when you first log into the console.

+   1. Locate the link called RDS. Click the link, which takes you to the RDS dashboard.
+   2. On the RDS dashboard, click the big Create Database button.
+   3. You should see a list of databases. Although Amazon RDS supports different database engines, select PostgreSQL and click Option. This displays the database fields.

You should now see a screen with three template options: a production database, a dev/test database, and a free tier. Select the Free Tier option. Next, under Settings, add the necessary information about our PostgreSQL database, setting the master user ID and password that we’ll use to log in to the database. Figure 12.3 shows this screen.