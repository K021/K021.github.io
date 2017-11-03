---
layout: post
title:  "EB Deploy: .ebignore 없이 .gitignore에 있는 파일 배포하기"
date:   2017-09-13 13:10:26 +0900
tags: git git/remote_branch
categories: AWS
---

## AWS Elastic Beanstalk Deploy: .ebignore 파일 없이 .gitignore에 있는 파일 배포하기  
>  .gitignore에 있는 파일을 배포 하면서 동시에 git commit 단위로 배포할 수 있다

AWS의 EB CLI는 기본적으로 git의 commit 단위로 배포를 해준다.  
프로젝트 폴더 안에서 커밋된 내용을 배포하고, 그렇지 않은 파일은 배포하지 않는 것.  
그러나 커밋되지 않은 파일 중에 배포해야 하는 파일이 있다. 예를 들면 `AWS_ACCESS_KEY_ID` 같은 것들.

해결책을 구글링 해보면, .ebignore 파일을 만들라고 한다. 
그러나 이 방법에는 치명적인 단점이 있는데, 배포가 커밋단위로 관리되지 않고, .ebignore에 없는 내용은 전부 올라가버린다는 것이다. (.ebignore 파일이 있으면 커밋단위 뿐 아니라 .gitignore도 무시한다. 그래서 .gitignore의 파일을 전부 복사하고 내용을 추가해야 한다.)



해결책이 하나 있다. `eb deploy --staged` 옵션을 사용하는 것이다.  
_commit 하면 안 되지만, deploy는 해야 하는 파일_은 

1. 강제로 `staged` 시킨다: `git add -f <file_name>`
2. 배포한다: `eb deploy --staged`
3. 다시 `unstage`: `git reset HEAD <file_name>*`
