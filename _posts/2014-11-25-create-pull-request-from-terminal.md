---
layout: post
title: Create a Github Pull Request from the terminal
category: terminal
---

To simply create a Github Pull Request on the site from the current active directory use this snippet

{% highlight bash %}
 function git_pull_request()
 {
    local NAME=`git rev-parse --abbrev-ref HEAD`
    local PATH_REPO=`git remote show -n origin | grep Push | cut -d: -f2- | cut -d/ -f 1- | tr -d " "  | cut -d. -f-2`
    git push origin $NAME
    echo $PATH_REPO
    open $PATH_REPO/compare/$NAME
}

{% endhighlight %}


You can probably find a more up to date version in my [dotfiles](https://github.com/genintho/dotfiles)


