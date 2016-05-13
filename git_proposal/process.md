#Gitによるソース管理及びリリースフロー

現在のリリースフロー

```
(実装者作業)
 git clone {repository_name}
 git branch {ticket_num branch_name}
 git checkout {ticket_num branch_name}

 # code revision

 git commit -m '{ticket_num} hogehoge'
 git push origin {ticket_num new_branch}

（reviewer作業）
 # code review (maybe)
 
 git merge {ticket_num new_branch} {master}

(リリース担当者作業)
 git checkout master
 git pull master

 # Eclipse open
 # Force.com Online
 # Ant build/deploy (to salesforce)
```

recommended release procedure

```
(実装作業)

（最初だけ）

 git fork repository (button on github only)

 git clone {user's repository_name}
 git branch {anything}

 # code revision
 
 git commit branch_name
 git push branch_name

 # send pull request to release branch

 # review
 # accept & merge

 # create tag & release 
```
