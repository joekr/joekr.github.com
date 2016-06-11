+++
date = "2013-05-21T22:11:00-05:00"
title = "jenkins and bitbucket"
categories = ["Jenkins", "Bitbucket", "Git", "Software Development", "SSH"]
tags = ["Jenkins", "Bitbucket", "Git", "Software Development", "SSH"]
slug = "2013/jenkins-and-bitbucket"
+++

Tonight I had to setup a new instance of Jenkins. Having never set  Jenkins to work with Bitbucket before I thought this might help out others. This will cover Jenkins accessing only Bitbucket, having it deploy might be another post, but that isn't covered here.

## Jenkins install

Starting out, I have a blank [Digital Ocean](https://www.digitalocean.com/) droplet. There is basically nothing. I had to install Jenkins, which is pretty basic:

> wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -


> sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'


> sudo apt-get update


> sudo apt-get install jenkins

This will create the jenkins user and start up the Jenkins servlet on port 8080. If you need to setup Jenkins other ways checkout the [How-to](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).

<!-- more -->

## Jenkins Unix/security setup

Next I needed to setup Jenkins to use my unix user permissions. The security settings can be accessed by going to: [http://your_server:8080/configureSecurity/](http://your_server:8080/configureSecurity/)

From the configureSecurity page select "__Unix user/group database__"
click on "__Advanced__" and add "__sshd__" as the Service Name. While setting this up I got on error saying the jenkins user could read from "__/etc/shadow/__". So I had to add the jenkins user to the shadow group.

> usermod -G shadow jenkins

Lastly to setup permissions I selected: "Logged-in users can do anything".

## Git setup

Since this was a basic ubuntu install I had to install git:

> sudo apt-get install git

Then setup the git user information:

> git config --global user.email "example@email.com"


> git config --global user.name "jenkins"

Not having the user info set resulted in Jenkins later through throwing: __hudson.plugins.git.GitException: Could not apply tag jenkins-Pactsafe_-_TEST-2__ when trying to run a build. Better set them now. I used the global config because this droplet will only be used for Jenkins.

Next install the git plug in for Jenkins. Using the Jenkins menu go to:

1. Manage Jenkins
2. Manage Plugins
3. Available
4. Then search for "Git Plugin"

Once Jenkins restarts you can move on to setup a new job.

## New Job with SSH and Bitbucket

Finally, setting up Jenkins to simply access a repo from Bitbucket! They have a nice write up on this: [SSH with Bitbucket](https://confluence.atlassian.com/display/BITBUCKET/Using+the+SSH+protocol+with+bitbucket#UsingtheSSHprotocolwithBitbucket-RepositoryURLformatsbyconnectionprotocol)

Since Digital Ocean allows for shell access I was able to generate an ssh key for my jenkins user. I logged in as jenkins and ran **ssh-keygen**. ssh-keygen will (by default) generate the public key in ~/.ssh/id_rsa.pub. For more information checkout [Bitbucket's How-to](https://confluence.atlassian.com/pages/viewpage.action?pageId=270827678)

Open Bitbucket and go to the repository you want Jenkins to access. As Admin, access the repository's settings and setup a new [deployment key](http://blog.bitbucket.org/2012/06/20/deployment-keys/) using the previously generated contents from the id_rsa.pub file.

You will also need to setup a ~/.ssh/known_hosts file. If you don't the build will fail with this error __stderr: Host key verification failed.__

The easiest way to create the known_host file is to run (as jenkins):

> git ls-remote -h git@bitbucket.org:person/projectmarket.git HEAD

Doing this will prompt you to add Bitbucket as a known host. Once you confirm the host the key for bitbucket.org it will be added to the ~/.ssh/known_hosts file and you shouldn't get this error anymore.

## Run The Job

With all of that setup you should be able to run the job and receive a "Finished: SUCCESS" message in the Console Output.

There are many things that still need to be setup after this, but this gets Jenkins and Bitbucket playing together.
