---
layout: page
title: Version Control with Git
subtitle: Remotes in GitLab
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Explain what remote repositories are and why they are useful.
> *   Clone a remote repository.
> *   Push to or pull from a remote repository.

Version control really comes into its own
when we begin to collaborate with other people.
We already have most of the machinery we need to do this;
the only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.
In practice,
though,
it's easiest to use one copy as a central hub,
and to keep it on the web rather than on someone's laptop.
Most programmers use hosting services like
[GitHub](http://github.com),
[BitBucket](http://bitbucket.org) or
[GitLab](http://gitlab.com/)
to hold those master copies;
we'll explore the pros and cons of this in the final section of this lesson.

Let's start by sharing the changes we've made to our current project with the world.
Log in to GitLab,
then click on the icon in the top right corner to create a new repository called `planets`:

![Creating a Repository on GitLab (Step 1)](fig/gitlab-create-repo-01.png)

Name your repository "planets" and then click "Create Repository":

![Creating a Repository on GitLab (Step 2)](fig/gitlab-create-repo-02.png)

As soon as the repository is created,
GitLab displays a page with a URL and some information on how to configure your local repository:

![Creating a Repository on GitLab (Step 3)](fig/gitlab-create-repo-03.png)

This effectively does the following on GitLab's servers:

~~~ {.bash}
$ mkdir planets
$ cd planets
$ git init
~~~

Our local repository still contains our earlier work on `mars.txt`,
but the remote repository on GitLab doesn't contain any files yet:

![Freshly-Made GitLab Repository](fig/git-freshly-made-gitlab-repo.svg)

The next step is to connect the two repositories.
We do this by making the GitLab repository a [remote](reference.html#remote)
for the local repository.
The home page of the repository on GitLab includes
the string we need to identify it:

![Where to Find Repository URL on GitLab](fig/gitlab-find-repo-string.png)

Click on the 'HTTPS' link to change the [protocol](reference.html#protocol) from SSH to HTTPS.

> ## HTTPS vs SSH {.callout}
>
> We use HTTPS here because it does not require additional configuration.
> After the workshop you may want to set up SSH access, which is a bit more
> secure, by following one of the great tutorials from
> [GitHub](https://help.github.com/articles/generating-ssh-keys),
> [Atlassian/BitBucket](https://confluence.atlassian.com/display/BITBUCKET/Set+up+SSH+for+Git)
> and [GitLab](https://about.gitlab.com/2014/03/04/add-ssh-key-screencast/)
> (this one has a screencast).

![Changing the Repository URL on GitLab](fig/gitlab-change-repo-string.png)

Copy that URL from the browser,
go into the local `planets` repository,
and run this command:

~~~ {.bash}
$ git remote add origin https://gitlab.com/vlad/planets.git
~~~

Make sure to use the URL for your repository rather than Vlad's:
the only difference should be your username instead of `vlad`.

We can check that the command has worked by running `git remote -v`:

~~~ {.bash}
$ git remote -v
~~~
~~~ {.output}
origin   https://gitlab.com/vlad/planets.git (push)
origin   https://gitlab.com/vlad/planets.git (fetch)
~~~

The name `origin` is a local nickname for your remote repository:
we could use something else if we wanted to,
but `origin` is by far the most common choice.

Once the nickname `origin` is set up,
this command will push the changes from our local repository
to the repository on GitLab:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 821 bytes, done.
Total 9 (delta 2), reused 0 (delta 0)
To https://gitlab.com/vlad/planets
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
~~~

> ## Proxy {.callout}
>
> If the network you are connected to uses a proxy there is an chance that your last
> command failed with "Could not resolve hostname" as the error message. To
> solve this issue you need to tell Git about the proxy:
>
> ~~~ {.bash}
> $ git config --global http.proxy http://user:password@proxy.url
> $ git config --global https.proxy http://user:password@proxy.url
> ~~~
>
> When you connect to another network that doesn't use a proxy you will need to
> tell Git to disable the proxy using:
>
> ~~~ {.bash}
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~

> ## Password Managers {.callout}
>
> If your operating system has a password manager configured, `git push` will
> try to use it when it needs your username and password. If you want to type
> your username and password at the terminal instead of using
> a password manager, type:
>
> ~~~ {.bash}
> $ unset SSH_ASKPASS
> ~~~
>
> You may want to add this command at the end of your `~/.bashrc` to make it the
> default behavior.

Our local and remote repositories are now in this state:

![GitLab Repository After First Push](fig/gitlab-repo-after-first-push.svg)

> ## The '-u' Flag {.callout}
>
> You may see a `-u` option used with `git push` in some documentation.
> It is related to concepts we cover in our intermediate lesson,
> and can safely be ignored for now.

We can pull changes from the remote repository to the local one as well:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
From https://gitlab.com/vlad/planets
 * branch            master     -> FETCH_HEAD
Already up-to-date.
~~~

Pulling has no effect in this case
because the two repositories are already synchronized.
If someone else had pushed some changes to the repository on GitLab,
though, this command would download them to our local repository.

> ## GitLab Timestamp {.challenge}
>
> Create a remote repository on GitLab.
> Push the contents of your local repository to the remote.
> Make changes to your local repository and push these changes.
> Go to the repo you just created on GitLab and check the [timestamps](reference.html#timestamp) of the files.
> How does GitLab record times, and why?
