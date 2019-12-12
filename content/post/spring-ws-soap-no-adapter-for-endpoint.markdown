+++
date = "2012-09-26T20:40:00-05:00"
title = "Spring-ws SOAP 'No adapter for endpoint'"
categories = ["Spring", "SOAP", "Software Development", "Java"]
tags = ["Spring", "SOAP", "Software Development", "Java"]
slug = "2012/spring-ws-soap-no-adapter-for-endpoint"
+++
Why am I using SOAP? I don't want to talk about it. Basically I have a need for a Spring MVC project to provide a Spring-ws SOAP endpoint.
After following the Spring documentation [here](http://static.springsource.org/spring-ws/sites/2.0/reference/html/tutorial.html#d5e190) and downloading the source code [here](http://static.springsource.org/spring-ws/sites/2.0/downloads/releases.html) I noticed something. The documentation is using jdom for marshalling. While the source code is using jdom2.

After struggling for a while I find out that using jdom, even with everything setup correctly, will result in the following error:

    'java.lang.IllegalStateException: No adapter for endpoint'

This, I guess, is a [known issue](https://jira.springsource.org/browse/SWS-782?page=com.atlassian.jira.plugin.system.issuetabpanels:changehistory-tabpanel). The thing to do is use jdom2, or something else like JAXB, Castor, etc.

I know documentation is hard to keep up-to-date. I just thought I should put something else out on the net incause others have the same issue.
