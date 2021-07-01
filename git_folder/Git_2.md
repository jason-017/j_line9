# Git 추가 설명

## 1.commit

> commit을 통해 이력을 확정하면 hash 값이 부여되며, 이 값을 통해 동일한 커밋인지를 확인한다.

```bash
# WD x , staging area 변화 x
# 변경사항 x
$ git commit
nothing to commit, working tree clean
# WD o , staging area 변화 o
Untracked files:
        lee.txt

nothing added to commit but untracked files present

```

### commit 메시지 작성하기

```bash
부제 vim 활용법
$ git commit
```

* 편집모드(i)
  * 문서 편집 가능
* 명령모드(ESC)
  * dd : 해당 줄 삭제
  * wq : 저장 및 종료
    *  w : write(저장)
    * q : quit(종료)
  * :q! : 강제종료
    * q : quit
    * ! : rkdwp

# log 활용하기

```bash
$ git log
$ git log --oneline
$ git log -1
$ git log -1 --oneline
$ git log --oneline --graph
```

* HEAD : 현재 작업하고 있는 커밋 이력 및 브랜치에 대한 포인터

```bash
a4317b0 (HEAD -> master) Add lee.txt
# 나는 현재 amster 브랜치에 있고,
# a4317b0 커밋의 상태에 있다.
```

* 예시)

```bash
a4317b0 (HEAD -> master) Add lee.txt
e97ba41 (feature/signout) complete signout
e980006 (origin/master) 집이당
# 나는 master 브랜치에서 a4317b0 커밋을 했고,
# feature/signout 브랜치는 e97ba41 이력이고,
# 원격저장소 (origin/master)는 e980006 이력이다.

```

# 직전 커밋 메시지 수정

> 아래의 명령어는 **커밋 이력을 변경**하기 때무에 조심해야 한다. 공개된 저장소에(원격 저장소)이며 push된 이력이라면 절대 해서는 안된다.

```bash
$ git commend --amend
```

### 커밋시 특정 파일을 빠뜨렸을 때

만약, staging area에 특정 파일('omit_file.txt)을 올리지 않아서 커밋이 되지 않았을 때!

```bash
$ git add omit_file.txt
$ git commit --amend
```

## 2. staging area

* 커밋 이력이 파일 수정한 경우

```bash
$ git status
On branch master # 마스터 브랜치에 있다.
Your branch is ahead of 'origin/master' by 10 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit: # staged가 하단 변경 사항들
	# 커밋이 되기 위해서 -> staged로 바꾸려면
  (use "git add <file>..." to update what will be committed)
  	# WD에 있는 변화를 버리려면 -> 고양이
  	# (커밋 이후에 변경된 사항을 없에버렷)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2.txt
# staging area 가 비어있습니다.
# (커밋에 추가될 변화가 없다.)
no changes added to commit (use "git add" and/or "git commit -a")

```

```bash
$ git add 2.txt
$ git status
On branch master
# 커밋이 될 변화
# (커밋 명령어 했을 때, 아래의 내용이 이력에 남는다.)
Changes to be committed:
	# 
  (use "git restore --staged <file>..." to unstage)
        modified:   2.txt
```



* 커밋 이력이 없는 파일인 경우

```bash
$ git status
On branch master
# tracking 되고 있지 않는 파일 -> commit(이력)에 한번도 관리된적 없다.
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt
# 커밋할 것도 없고(staging area가 비어있고),
# 트래킹 되고 있지 않능 파일도 있다.
nothing added to commit but untracked files present (use "git add" to track)
```

### add 취소하기

```bash
$ git restore --staged <file>
```

* 구버전 git에서는 아래의 명령어를 사용해야 한다.

* ```bash
  $ git reset HEAD <file>
  ```

### Working directory 변화 삭제하기

> git에서는 모든 commit된 내용을 되돌릴 수 있다. 다만 아래의 WD 내용을 삭제하는 것은 되돌릴 수가 없다.

```bash
$ git restore <file>
```

* 구버전 git에서는 아래의 명령어를 사용해야 한다.

* ```bash
  $ git checkout -- <file>
  ```

## Stash

> Stash는 변경사항을 임시로 저장 해놓는 공간이다.

### 예시 상황)

1. feature branch에서 a.txt.를 변경 후 커밋
2. master branch에서 a.txt를 수정!(add/ commit x)
3. merge

```bash
$ git merge test
# 현재 merge 명령어로 인해 아래의 파일이 덮어쓰여질 수 있다.
error: Your local changes to the following files would be overwritten by merge:
        a.txt
# commit 을 하걱나 -> 이력 확정을 해서 merge시 충돌 나는 상황으로!
# stash 해서 -> working directory 를 잠시 비워 놓음
Please commit your changes or stash them before you merge.
Aborting
Updating 1b1b4df..bf1945a


```

## 명령어

```bash
$ git stash # stash 공간에 저장
Saved working directory and index state WIP on master: 1b1b4df a.txt test

$ git stash list # stash 공간 내용 확인(목록)
stash@{0}: WIP on master: 1b1b4df a.txt test

$ git stash pop # stash 공간에서 적용(apply)하고 목록에서 삭제(drop)하기
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (0a99c5cf4a4b0729e671e645b294a42b96ad65cb)

```

## 예시 상황 해결

```bash
$ git stash
Saved working directory and index state WIP on master: 1b1b4df a.txt test

$ git status
On branch master
nothing to commit, working tree clean

$ git merge test
Updating 1b1b4df..bf1945a
Fast-forward
 a.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
 
$ git stash pop
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
The stash entry is kept in case you need it again.

# 충돌 해결 후 작업 진행
<<<<<<< Updated upstream
Test 브래치 내용
=======
Master 수정 중임...
>>>>>>> Stashed changes

```

## reset vs revert

> commit 이력을 되돌리는 작업을 한다.

* reset : 이력을 삭제한다.

  * --mixed : 기본 옵션, 해당 커밋 이후 변경 사항 staging area 에 보관
  * --hard : 해당 커밋 이후 변경사항 모두 삭제 **주의**
  * --sort: 해당 커밋 이후 변경사항 및 working directory 내용까지 모두 보관

  ```bash
  $ git log --oneline
  8669a25 (HEAD -> master) a.txt
  bf1945a (test) a.txt test
  1b1b4df a.txt test
  f1f7360 a.txt
  
  ```

  

* revert : 되돌렸다는 이력을 남긴다.

  ```bash
  $ git log --oneline
  6b6b5d0 (HEAD -> master) Revert "Revert "a.txt test""
  0577ffa b.txt
  d163440 Revert "a.txt test"
  8669a25 a.txt
  bf1945a (test) a.txt test
  1b1b4df a.txt test
  f1f7360 a.txt
  
  ```

  