## 1.Github 사용법

------

1. 커멘드라인에 깃 설치

```
sudo apt-get install git
```

2. 프로젝트 파일을 만들기.

```
`mkdir` `gitfth`
```

3. git의 (버전) 저장소를 만들기.

```
git init	#ls -al로 .git 있는지 확인.
```

4. git이 관리할 대상으로 파일 등록

 git은 기본적으로 새로운 파일 관리하지 않음, 따라서 파일 관리 위해 관리대상으로 등록 필요. 

```
vi f1.txt	# 파일 생성.
git add f1.txt	# git이 파일을 추적하도록 명령
git status	# 프로젝트 폴더의 상태를 확인.
```

5. git 버전 만들기(commit)

>  버전에 포함될 버전을 만든 사람에 대한 정보를 설정합니다. 이 설정은 ~/.gitconfig 파일에 저장되고 1번만 해주면 됩니다. 
>
> ```
> git config --global user.name "자신의 닉네임"
> git config --global user.email "자신의 이메일"
> ```

6. 이미 버전관리 되고있는것은 바로 commit 하면 안됨.

   버전관리 시스템에 add 해야 함

   1) git에서 새로운 파일이 생겼을 때, 그 파일에 대한 버전관리를 깃에게 명령할 때에도 add를 쓴다.

   2)이미 버전관리 되어있을때에 파일이 수정되었을 때에도 add 한다.

   -> 파일이 수정될 때나, 맨 처음 등록할 때에도 add

7. 왜 git이 add라는 프로세스를 가지는가?

   1) 여러개의 작업들을 담은 거대한 버전 하나를 만들 때(커밋 시기 놓쳤을 때)

   2) 이때 깃은 커밋하고자 하는 파일만 커밋 할 수 있다.

   3) git add <file명> : file명에 해당하는 파일을 statge(commit 대기상태)에 올림에 둠

   4) stage(commit 대기 파일들이 가는 곳) -> repository(commit 결과가 갈 곳)



8. git 변경사항 확인하기

   ```
   # 로그에서 출력되는 버전 간의 차이점 출력
   
   git log -p
   
   # git log <commit id> : commit id 이전 commit log 값들 보기
   
   git log bbe6b3e0172b6a59e9ade2c48e228b53d7655eba

   # 버전 간의 차이점을 비교할 때
   
   git diff "버전 id".. "버전 id2"
   		  <이전 버전>..<이후 버전> : 이전f1.txt 를 f3.txt로 cp했을 때, 
   
   index : f1.txt와 f3.txt는 같은 objects를 가르키고 있다.
   
   깃은 어떤 파일을 저장할 때, 파일의 이름이 달라도, 파일의 내용이 같으면 같은 objects파일을 가르킨다.
   
   이전은 -- 이후는 ++, 반대도 가능하나 반대로 +, - 나옴
   		  <이후 버전 commit id >435fedd450a37f6d922fd80f71d62421783c5260
   		  <이전 버전 commit id > 161c9b1273196416512f2c0a70b256bf17abc178
   
   # git add 하기 전과 add 한 후의 파일 내용 비교할 때
   
   git diff
   ```
   
   
   

   ### 아래 명령은 버전 id로 돌아가는 명령입니다.
   git reset --hard "버전 id" 
   (hard는 위험한 방법임. 또 reset은 local에서만 쓰고 repository 여러명이 쓰고있는 깃 원격 저장소에서는 쓰면 절대 안됨)

   ### 버전 id의 커밋을 취소한 내용을 새로운 버전으로 만드는 명령
05688f6ed52f4a169649b102860388254ab4ee72   git revert "버전 id"

   10. git commit -a (add하고 commit 동시에 하는 법)
11. git commit -am <msg> (에디터 띄우지 않고 바로 커밋, commit message로 사용)



## 2. 깃의 원리

------

### 2-1. gistroy 설치

> **gistory란?**
>
> gistory는 git을 분석하기 위한 도구입니다.명령을 내렸을 때 git의 내부에서는 어떤 일이 일어나는가를 분석하면서 git이 어떻게 동작하는가를 스스로 공부하는데 도움을 드리기 위해서 고안된 도구입니다. 
>
> <u>1) .git 디렉토리 안에 메커니즘은 어떻게 돌아갈까?</u>
>
> **gistory 설치**
>
> ```
> $ pip3 install gistory
> $ cd ./git
> $ gistory # 실행
> # http://0.0.0.0:8805/
> ```

### 2.2. git add 하면 **objects**라는 디렉토리와 **index** 디레고리 생김.

> **objects** : f1.txt라는 내용이 담겨있음
>
> **index** : 파일의 이름을 담음 : <objects>/f7/0f10e4db19068f79bc43844b49f3eece45c4e8
>
> ```
> 100644 ee5badcf9918e7ed76d66bc3e978a1313a31c219 0	crawling.py # 각각의 파일명이 적혀있음
> 100644 f70f10e4db19068f79bc43844b49f3eece45c4e8 0	f1.txt # objects의 디렉토리
> ```



### 2.3 index : f1.txt를 f3.txt로 cp했을 때

> ***f1.txt와 f3.txt가 같은 objects를 가르키고 있다.***
>
> <u>***깃은 파일을 저장할 때, 파일의 이름이 달라도, 파일의 내용이 같으면 같은 objects를 가르킨다.***</u>
>
> ```
> 100644 ee5badcf9918e7ed76d66bc3e978a1313a31c219 0 crawling.py 
> 100644 f70f10e4db19068f79bc43844b49f3eece45c4e8 0	f1.txt 
> 100644 f70f10e4db19068f79bc43844b49f3eece45c4e8 0	f3.txt
> ```



### 2.4 objects 파일명의 원리

(내용이 같으면 파일의 이름이 같다 : 메커니즘?)

> http://www.sha1-online.com/ : hash 알고리즘 통과 결과 보기

> #### 1) git은 내부적으로 SHA1이라는 hash알고리즘을 통과시켜 파일의 이름을 도출
>
> 이 파일의 이름 f70f10e4db19068f79bc43844b49f3eece45c4e8 
>
> 두 글자를 때어서 objects/??/  디렉토리 부분 만듬
>
> 그 밑에 0f로 시작하고 e8로 끝나는파일을 만들어서 그 안에 hello라는 정보를 저장한다.
>
> #### 2) **Git의 내부 적용**(git add 시)
>
> 1) f1.txt의 A라는 내용을 봐서, 내용과 부가적 정보를 압축한다.
>
> 2) 이 압축 결과를 **<u>SHA1 Hash 알고리즘을 통과시켜</u>** 나온 값
>
> ​     (ex. f70f10e4db19068f79bc43844b49f3eece45c4e8)
>
> 3) 이 값을 통해 objects(??/start/end/)라는 디렉토리와 파일을 만든다.
>
> 4) 그 안에 A라는 정보를 저장한다.

## 2.5 commit의 원리

5가지 파일 생성

.obejcts/ : 버전도 objects안에 들어감

tree **56fe43fe14ef81fa468117877bd7fc26369fab51**(objects 명)

> 100644 blob ee5badcf9918e7ed76d66bc3e978a1313a31c219	crawling.py
>
> 100644 blob f70f10e4db19068f79bc43844b49f3eece45c4e8	f1.txt 
>
> 100644 blob b1e67221afe8461efd244b487afca22d46b95eb8	f3.txt		

[1st commit] tree 만 존재

100644 blob 0510148818527aafeb8d2a19b00a1ed13136dc6e	crawling.py
100644 blob f70f10e4db19068f79bc43844b49f3eece45c4e8	f1.txt
100644 blob 130148a68520629a3c4f9b91dfb032e32b039d3a	f2.txt
100644 blob b1e67221afe8461efd244b487afca22d46b95eb8	f3.txt

```
[2nd commit] 

blob : 파일의 내용을 담고있음

tree : blob에 대한 정보를 담는 tree

commit : 이전commit 정보인 parent 존재.

각각의 버전마다 서로 다른 tree가 있음 : snap shot 각각의 버전은 그 버전이 만들어낸 시점 objects들을 담고 있음

[commit] 첫 번째 커밋
```

`tree b41fc1f370169caed1e0dda9f5f29954e9cc927c parent 4856750e618de4f0aaf93ee4fbc38334b78ae7b0 author yeoineon  1574927629 +0900 committer yeoineon  1574927629 +0900 `
```

[commit] 두번째 커밋 

```
`tree 206bf81c16bd26ae3eed678a55124b3f50c211d3 parent 9c0edd83c7ca200390ef643edcba6d70fc030cbb author yeoineon  1574928198 +0900 committer yeoineon  1574928198 +0900 version6`
```

[tree] 206bf81c16bd26ae3eed678a55124b3f50c211d3

```
`100644 blob 0510148818527aafeb8d2a19b00a1ed13136dc6e	crawling.py 040000 tree 4db322bc1bab3860ae1f5776314b4c5d5c592a21	d1 100644 blob f70f10e4db19068f79bc43844b49f3eece45c4e8	f1.txt 100644 blob 130148a68520629a3c4f9b91dfb032e32b039d3a	f2.txt 100644 blob b1e67221afe8461efd244b487afca22d46b95eb8	f3.txt`
```

[tree] 4db322bc1bab3860ae1f5776314b4c5d5c592a2

```
`100644 blob f70f10e4db19068f79bc43844b49f3eece45c4e8	f1.txt`




## **2.6 index는 무엇인가?, status 원리**

[index] index

`100644 0510148818527aafeb8d2a19b00a1ed13136dc6e 0	crawling.py 100644 f70f10e4db19068f79bc43844b49f3eece45c4e8 0	d1/f1.txt 100644 f70f10e4db19068f79bc43844b49f3eece45c4e8 0	f1.txt 100644 130148a68520629a3c4f9b91dfb032e32b039d3a 0	f2.txt 100644 b1e67221afe8461efd244b487afca22d46b95eb8 0	f3.txt`

```

가장 최신 commit의 tree를 클릭해 봐서 index와 같으면 commit 할 것이 없는 상태

**git status**

```


## 3. git branch

git log : commit 정보 보기

git log -p : commit 정보 및 코드 추가 정보 보기





### 브랜치 목록보기

git branch

### 브랜치를 생성할 때 : 현재 마스터브랜치의 상태를 그대로 가지게 됨

git branch "새로운 브랜치 이름"

### 브랜치를 삭제할 때

git branch -d

### 병합하지 않은 브랜치를 강제 삭제할 때 

git branch -D

### 브랜치를 전환(체크아웃)할 때

git checkout "전환하려는 브랜치 이름"

### 브랜치를 생성하고 전환까지 할 때 

git checkout -b "생성하고 전환할 브랜치 이름"




## 3-1. branch 정보 확인

브랜치를 만들면, 상당한 복잡성을 가진다. (=복잡성 때문에 힘듬)

<u>**브랜치 간에 비교할 때**</u>

git log "비교할 브랜치 명 1".."비교할 브랜치 명 2"

git log exp..master : exp있고 master에는 없는 것 (git log -p exp..master)

git log master..exp : master에는 있고 exp는 없는 것

<u>**브랜치 간의 코드를 비교 할 때**</u> 

git log -p exp..master 

git diff "비교할 브랜치 명 1".."비교할 브랜치 명 2"

> <u>**1) 모든 브랜치 보기**</u>
>
> git log --branches
>
> **<u>2) master와 branch들의 최근 commit 상태 보기(head라는 파일 존재)</u>**
>
> git log --branches --decorate ***보기**
>
> **<u>3) master와 branch들의 최근 commit과 그래프 보기</u>**
>
> git log --branches --decorate --graph***보기**
>
> <u>**4) 로그에 모든 브랜치를 표시하고, 그래프로 표현하고, 브랜치 명을 표시하고, 한줄로 표시할 때**</u> 
>
> git log --branches --graph --decorate --oneline



 **A 브랜치로 B 브랜치를 병합할 때 (MASTER ← B) **:이것도 커밋

git checkout MASTER
git merge B

**$ git log --branches --graph --decorate --oneline**

```
*   3143928 (HEAD -> master) Merge branch 'exp'
    |\  
    | * 05688f6 (exp) 4
    | * 3e08d1f 3
*   | f8524b3 5
    |/  
*   c090483 2
*   1114f6e 1
```

### exp와 master 똑같이 할 때

```
* eebe966 (HEAD -> exp, master) 6
*   3143928 Merge branch 'exp'
|\  
| * 05688f6 4
| * 3e08d1f 3
* | f8524b3 5
|/  
* c090483 2
* 1114f6e 1

```







# 브랜치 충돌

1) 파일이 다르면 자동으로 병합 됨.

2) 파일이 같으면 문제가 생김!

​	2-1) common.py에 master와 branch가 수정부분이 겹치지 않으면 merge된다.

```
master의 common.txt
functionC(){
}
functionA(){
}

exp의 common.txt
functionA(){
}
functionB(){
}


$ git merge exp
functionB(){
}
functionA(){
}
functionC(){
}
```

​	2-2) master와 branch가 수정부분이 같으면			

```
yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ git merge exp
자동 병합: common.txt
충돌! (내용): common.txt에 병합 충돌
자동 병합이 실패했습니다. 충돌을 바로잡고 결과물을 커밋하십시오.
yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ 
yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ 
yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ vi common.txt 
yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ git status
현재 브랜치 master
병합하지 않은 경로가 있습니다.
  (충돌을 바로잡고 "git commit"을 실행하십시오)
  (병합을 중단하려면 "git merge --abort"를 사용하십시오)

병합하지 않은 경로:
  (해결했다고 표시하려면 "git add <파일>..."을 사용하십시오)

	양쪽에서 수정:  common.txt

커밋할 변경 사항을 추가하지 않았습니다 ("git add" 및/또는 "git commit -a"를
사용하십시오)

```

```
functionB(){
}
<<<<<<< HEAD           : 현재 위치 master
functionA(master){     : master 부분
=======                : 구분자를 중심으로 HEAD(현재 chekcout 한 브랜치의 수정사항)
functionA(exp){        : exp 부분
>>>>>>> exp            : EXP 브랜치의 내용
}
functionC(){
}


1) 수정한다
functionB(){
}
functionA(master,exp){
}
functionC(){
}

2) git add common.txt
3) git status
4) git commit
```





## 4. stash 

- 다른 브랜치로 checkout을 해야 하는데 아직 현재 브랜치에서 작업이 끝나지 않은 경우는 커밋을 하기가 애매합니다. 이런 경우 stash를 이용하면 작업중이던 파일을 임시로 저장해두고 현재 브랜치의 상태를 마지막 커밋의 상태로 초기화 할 수 있습니다. 그 후에 다른 브랜치로 이동하고 작업을 끝낸 후에 작업 중이던 브랜치로 복귀한 후에 이전에 작업하던 내용을 복원할 수 있습니다. 여기서는 이 기능에 대해서 알아봅니다. 

- git stash --help

- 임시 저장

  ```
  $git stash
  $git stash save
  ```

- 가장 최신상태로 복원

  ```
  $git reset --hard HEAD
  $git reset --hard 
  $git status
  ```

- 리스트 보기

  ```
  $git stash list
  <-- git stash 내용은 지우지 않는이상 살아있음.
  
  
  yeo@yeo-VirtualBox:~/workspace/github_practice/gitbranch$ git stash list
  stash@{0}: WIP on exp: 5484be5 1   <--- 이게 더 최신 것 : apply시 적용 됨
  stash@{1}: WIP on exp: 5484be5 1
  
  $git stash apply
  $git stash drop
  $git stash apply
  $git stash drop 
  -> 한번에 하기 : git stash pop
  ```

- 감춰놓은 내용 복원

  ```
  git stash apply
  ```

- git stash는 최소한 버젼관리가 되고있는 파일에 대해서만 해야함

  -> 안그러면 적용 안됨.(따라서 add는 해야 함....)







원격 저장소

```
# 원격저장소를 직접 만들 때에는 git init --bare 옵션을 써야 함
$git init --bare remote
# .git 파일안의 내용이 생김
# 수정, 작업이 불가능 함

# 로컬 저장소 .git 있는 장소
$git remote add origin /home/yeo/workspace/github_practice/git/remote
# origin이라 부르겠따.

# git remote -v
origin	/home/yeo/workspace/github_practice/git/remote (fetch)
origin	/home/yeo/workspace/github_practice/git/remote (push)

# 저장소 지우기
git remote remove origin

# 옵션
git config --global push.default simple

# 로컬과 원격 연결시켜주는 것
$git push --set-upstream origin master 
 #현재 local브랜치와 원격 브랜치 사이에 master브랜치로 푸시 하겠다.
```





6. github

- github에서 받아오기
- git log --reverse 거꾸로 로그 봄

```
@ git remote add origin master <원격저장주소>
-> 원격저장소 등록
@ git remote -v
-> 원격저장소 보기
@ git push -u origin master :
-> 앞으로 현재 브랜치를 원격저장소 origin의 master에 동기화하겠다.
@ git remote remove <원격저장소이름>
-> 원격저장소로 등록한 이름과 주소 삭제
```





git remote --help

### 현재 로컬저장소에 원격저장소(remote 리퍼지토리를) add 시킨다. ( 주소 이며 = origin 이라는 별명 부여 하겠다.)

```
$git remote add origin https://github.com/YeopIn/gitfth.git
$git remote add friend https://github.com/YeopIn/gitfth.git
```

- origin이라고 하는 원격 저장소가 만든 것을 확인

```
$ git remote
```

- 더 자세히 보기 위함

```
$ git remote -v

origin	https://github.com/YeopIn/gitfth.git (fetch)
origin	https://github.com/YeopIn/gitfth.git (push)
```

- 원격 저장소 지우기

```
$ git remote remove friend
$ git remote -v
```

- push

- #깃은 로컬 저장소를 기준으로 함
  로컬저장소에서 원격저장소로 나의 작업을 보낼 때 (PUSH)한다.

```
$git push -u origin master
username : 
password :

#-u : 로컬과 원격저장소 연결, 그다음은 commit 만 하면 됨.
```

# 원격에서 로컬 저장소로

```
$git clone https://github.com/YeopIn/gitfth.git .
```

