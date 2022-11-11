# 0x19. Postmortem

##Summary of the problem
The incident was reported on Thursday 10 November 2022 at 18:00 and ended on Friday 11 November 2022 at 10:22. The incident consisted of the shutdown of the Payara application server which hosts all the API services of the Post Office application. The incident affected 100% of the users of the application.
The incident is caused by a high number of resource requests on the application server.

The incident was detected on Thursday 10 November at 2pm by the technical team of the Post Office who could not connect to the mobile application. 
The SPT team checked and found that the Payara application server had stopped working. The service of this application server could no longer be started.
The incident was reported to me on Thursday, November 10 at 6 pm via Whatsapp.

After inspection of the server logs with the SPT IT team, we noticed this error (OutOfMemoryError: GC overhead limit exceeded).  This means that the processes of the deployed services are consuming more than the default memory allocated by Payara.

This error comes from the Java Virtual Machine (JVM) which is the runtime environment for Java applications.

The solution was to increase the default size allocated to the JVM. By default Payara allocates this size to 512Mega. We changed it to 2giga as recommended by Payara on production servers.

Our development team will analyze the code and check if there are objects that consume memory.
