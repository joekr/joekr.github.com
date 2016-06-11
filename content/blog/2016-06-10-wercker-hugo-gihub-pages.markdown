+++
date = "2016-06-10T21:14:00-05:00"
title = "Deploying Hugo site using wercker to github"
categories = ["go", "CI", "git"]
tags = ["go", "CI", "git"]
slug = "2016/deploying-hugo-site-using-wercker-to-github"
+++
After moving to Hugo from Jekyll I needed to setup auto-deploy changes to my github page. Originally it was set up to use a `deploy.sh` script from the [Hugo blog post](https://gohugo.io/tutorials/github-pages-blog/#deploy-sh). That just isn't good enough. I'm lazy and I want the computer to do the work for me.

### Wercker
Hugo has another great post, [automated deployments using wercker](https://gohugo.io/tutorials/automated-deployments/). Since I split up my source and site repos from the [previous Hugo post](https://gohugo.io/tutorials/github-pages-blog/#hosting-personal-organization-pages) for my personal page I had to make one tweak to the [wercker.yml](https://github.com/joekr/joekr.github.com-source/commit/c4541908a4f32c231c2624ee2d18c908ffa4fefa) file. I added `repo: joekr/joekr.github.com` to the deploy step. This pushes the statically generated site contents to that repo instead of the source.

I've made the [wercker application public](https://app.wercker.com/#applications/575b1ca084f3ec631a023506). The pipeline is pretty simple, but sharing it out might help others out. Wercker is using [joekr.github.com-source](https://github.com/joekr/joekr.github.com-source) to build and deploy to [joekr.github.com](https://github.com/joekr/joekr.github.com).

I'm not 100% sold on this two repo setup but for now it just works. Now all I have to do is commit this post! 👍
