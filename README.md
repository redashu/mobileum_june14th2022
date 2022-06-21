## training plan 

<img src="plan.png">

### Revision process of docker , kubernetes and CI 

<img src="rev.png">

### multi app container image 

<img src="multi.png">

### 3 sample html project 

```
 773  git clone https://github.com/microsoft/project-html-website.git
  774  ls
  775  git clone https://github.com/schoolofdevops/html-sample-app.git
  776  history 
  777  ls
  778  git clone https://github.com/yenchiah/project-website-template.git
  779  ls
```

### Dockerfile 

### info about apache httpd 

<img src="httpd.png">

### pushing project to git repo 

```
812  git  add  .
  813  git commit -m "ashu customer app v1 "
  814  git config --global user.email "ashutoshh@linux.com"
  815  git config --global user.name  redashu
  816  git commit -m "ashu customer app v1 "
  817  history 
[ashu@docker-client mobi-ashuapp]$ git status 
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
[ashu@docker-client mobi-ashuapp]$ 
```

### pushing it 

```
git push 
Enumerating objects: 136, done.
Counting objects: 100% (136/136), done.
Delta compression using up to 2 threads
Compressing objects: 100% (131/131), done.
Writing objects: 100% (135/135), 2.64 MiB | 7.40 MiB/s, done.
Total 135 (delta 7), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (7/7), done.
To https://github.com/redashu/mobi-ashuapp.git
   ac3d15f..0d3c7ad  master -> master
```
### CI using jenkins 

<img src="ci.png">


