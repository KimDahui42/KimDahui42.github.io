---
layout: post
title: "하루 계획(4)"
date: 2021-05-13
excerpt: "오늘 하루는 후회없이 살았나요?"
tags: [plan, life, study]
category : [Daily Plan]
comments: false
---
 ## 언젠가는 승부를 봐야지하고 벼르고 있던 fork한 repository 커밋로그 유지하면서 옮기기
 
 공부해야지하고 다짐한 이후로 거의 매일 깃허브에 커밋로그를 남기려했다. 블로그 관리가 가장 무난하게 깃허브 사용에 적응하도록 도와줄거라 생각해 jekyll 테마도 가져오고 뒤적였는데...
 뒤늦게 fork한 저장소에서 커밋은 잔디가 남지 않는다는 사실을 알게되었다 너무 충격이야..
 <br><br>
 중간에 오류가 나서 뭐가 잘못되었는지 생초보는 정신이 아득해졌음 결과적으로 해결했으니깐(기분이 좋아서 어떻게든 분출하고싶으니깐) 기록해보자
 
 * git bash에서 진행했음
 * 옮기고 싶은 저장소를 클론(기존 저장소 이름을 original.git이라하자) 
 ```
 git clone --mirror https://github.com/original.git
 cd original.git
 ```
 * 새로운 저장소 연결(새로운 저장소 이름을 destination.git이라하자) 
 ```
 git remote add destination https://github.com/destination.git
 //git remote -v로 연결된 저장소 목록을 확인할 수 있다.
 ```
 * push하기
 ```
 git push destination --mirror
 ```
 여기서 오류가 났다!!
 ```
*[new branch] destination/master -> destination/master
remote rejected main (refusing to delete the current branch: refs/heads/main
error: failed to push some refs to 'https://github.com/original.git'
```
브랜치에 문제가 있다는 것같은데.. 한참 이것저것 뒤적이다 master에서 master로 이어야하는데 main브랜치만 있다는 것을 발견했다. 저장소 설정에서 브랜치를 main에서 master로 변경하고 다시 시도하니 성공했다. 
뿌듯해요
