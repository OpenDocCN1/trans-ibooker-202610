ithub.io/pcc_3e; the updates there will almost certainly help you carry out a successful deployment.

# Understanding Deployments

When you’re trying to troubleshoot a particular deployment attempt, it’s helpful to have a clear understanding of how a typical deployment works. Deployment refers to the process of taking a project that works on your local system, and copying that project to a remote server in a way that allows it to respond to requests from any user on the internet. The remote environment differs from a typical local system in a number of important ways: it’s probably not the same operating system (OS) as the one you’re using, and it’s most likely one of many virtual servers on a single physical server.

When you deploy a project, or push it to the remote server, the following steps need to be taken:

+   - Create a virtual server on a physical machine at a datacenter.
+   - Establish a connection between the local system and the remote server.
+   - Copy the project’s code to the remote server.
+   - Identify all of the project’s dependencies and install them on the remote server.
+   - Set up a database and run any existing migrations.
+   - Copy static files (CSS, JavaScript files, and media files) to a place where they can be served efficiently.
+   - Start a server to handle incoming requests.
+   - Start routing incoming requests to the project, once it’s ready to handle requests.