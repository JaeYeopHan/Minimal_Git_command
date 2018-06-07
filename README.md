# Minimal_Git_command

Git 을 사용하면서 자주 사용하는 명령어들에 대해 정리한 repository 입니다.

</br>

## Basic

#### add

```
git add [file name]
```

#### 변경 사항이 있는 모든 파일 add

```
git add .
```

#### commit message 와 commit

```
git commit -m "[commit message]"
```

#### 변경 사항이 있는 모든 파일 add 와 동시에 commit

```
git commit -am "[commit message]"
```

#### remote repository 등록하기

```
git remote add [remote github repository address]
```

#### 특정 branch 생성하며 remote 에 push 하기

```
git push origin [branch name]
```

#### branch 간 이동하기

```
git checkout [branch name]
```

#### branch 생성하기

```
git checkout -b [branch name]
```

#### branch 이름 바꾸기

```
git branch -M [changed name]
```

#### 이전 commit 과 비교하여 파일 변경 사항 확인하기

```
git diff
```

#### commit log 보기

```
git log
```

#### git log graph 로 출력하기

```
git log --graph
```

---

</br>

## Useful command

#### 변경 사항이 있는 파일 변경 사항 취소하기

```
git checkout -- [file name]
```

#### commit 한 개 취소하기

```
git reset HEAD^
```

or

```
git revert HEAD
```

#### 바로 이전 commit message 수정하기

```
git commit --amend
```

#### local git repository 에 remote repository 을 upstream 으로 등록하기

```
git remote add --track master upstream [remote github repository address]
```

#### master branch 를 uptream 의 latest version 으로 update 시키기

```
git pull --ff upstream master
```

#### 여러 commit 을 하나로 합치기(squash)

```
git rebase -i HEAD~[commit 개수]
```

`.gitconfig`에서 설정해둔 editor 창이 나타난다. 기준이 될 맨 위의 한 commit 만 `pick`으로 두고 나머지는 `squash`라는 명령어로 바꿔준다. `:wq` 명령어를 통해 저장하고 종료한다. 그러면 또 다른 editor 창이 나타나는데, commit message 를 설정하는 editor 이다. 원하는 commit message 를 입력하고 `:wq` 명령어를 통해 저장하고 종료해주면 squash 가 된다.

#### git history 출력하기

```
git reflog
```

#### 바로 이전 commit 에 새로운 파일 변경 사항을 추가하기

```
git commit -C HEAD --amend
```

_cf> 만약 push 를 한 상태라면 `-f`옵션을 통해서 push 를 해줘야 한다. commit 을 새로 생성하지 않고 변경사항을 추가하는 것처럼 보이지만 내부적으로는 새로운 커밋이 생기는 것이기 때문에 push 된 commit 과 다른 commit 이다._

#### 파일의 변경 이력을 무시해서 stage 에서 임시로 제외하기

```
git update-index --assume-unchanged [파일명]
```

#### 파일의 변경 이력을 무시해서 stage 에서 임시로 제외한 상황을 되돌리기

```
git update-index --no-assume-unchanged [파일명]
```

#### 현재 branch 에서 변경 사항 커밋없이 저장해두기

```
git stash
```

#### 저장해두었던 변경 내역 불러오기

```
git stash pop
```

#### 다른 branch 의 특정 commit 가져와서 merge 하기

```
git cherry-pick [COMMIT_HASH_NUMBER]
```

</br>

---

</br>

## 상황에 따른 git command 집합

### 상황 1. conflict 가 발생하는 상황

여러 가지 방법이 있지만 그 중 두 가지.

```
git merge upstream/master
(resolve conflict in editor)
git add src
git commit
git rebase upstream/master
(resolve conflict in editor again)
git add src
git rebase --continue
git push -f upstream YOUR_BRANCH_NAME
```

or

```
git fetch upstream
git rebase upstream/master
(resolve conflict)
git add .
git rebase --continue
git push -f origin [YOUR_WORKING_BRANCH_NAME]
```

</br>

### 상황 2. 과거 commit 에 fix 한 commit 이 존재하여 이 commit 순서를 정렬하고 싶은 상황

어떤 commit 이 이미 올라갔는데 그 commit 에 해당하는 수정 사항이 발생하면 그에 해당하는 fix commit 을 추가할 경우가 있다. 이 때, commit 의 순서가 난잡해지는데 이 경우,`fixup`과 `autosquash` command 를 사용한다.

```
git commit --fixup [HASH]
```

이렇게 해서 해당 fix commit 을 추가해놓은 다음에

```
git rebase -i --autosquash [HASH]
```

위 명령어를 통해서 squash 해주면 커밋 순서도 정렬되면서 하나의 commit 으로 squash 된다. 위 명령어에서 `HASH`란 어느 커밋부터 squash 를 할 것인가를 의미한다.

_cf> fixup 과 squash 그리고 squeeze 의 차이점_  
`squash`는 새로운 인터페이스가 열리면서 commit message 도 수정(포함 또는 배제)할 수 있다. `fixup`은 commit message 를 `pick`한 commit 에 대해서만 적용한다. `squeeze`는 squash 하고자 하는 모든 commit message 를 포함하면서 squash 한다.

---

</br>

## 알아두면 쓸모있는 Git Tip

### Git command alias

`Git`을 설치한 디렉토리에는 `.gitconfig` 파일이 존재한다. 이 파일에서 자주 사용하는 명령어에 대해서 alias 를 지정해줄 수 있다.

```.gitconfig
[alias]
    g = git
    st = status
    co = checkout
    ad = add
    cm = commit -m
    acm = commit -am
    ph = push
    rb = rebase -i
    fh = fetch
    df = diff
    br = branch -a
    lg = log --graph --abbrev-commit --decorate --format=format:'%C(cyan)%h%C(reset) - %C(green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(yellow)%d%    C(reset)' --all
    re = reset HEAD\\^
    fu = fetch upstream
    rum = rebase upstream/master
    pom = push origin master
    list = config --get-regexp alias
    readme = !git add . && git commit -m "Update README.md" && git push origin master
    docs = !git add . && git commit -m "Update" && git push origin master
    update = !git fetch upstream && git rebase upstream/master && git push origin master
```

</br>

### Commit Log format

- 제목과 본문을 빈 행으로 분리한다
- 제목 행을 50 자로 제한한다
- 제목 행 첫 글자는 대문자로 쓴다
- 제목 행 끝에 마침표를 넣지 않는다
- 제목 행에 명령문을 사용한다
- 본문을 72 자 단위로 개행한다
- 어떻게 보다는 무엇과 왜를 설명한다

* 제목
  - 대문자, 동사로 시작. 앞에 'If applied, this commit will '가 생략된 것으로 간주
  - 가급적 50 자 안쪽
  - 마침표 생략
* 본문
  - 3 번째 줄부터 시작 (두번째 줄은 비움)
  - 최대줄 너비는 80 자. 이상은 줄바꿈

### 추천 동사(personal)

- `Add` : 새로운 기능 또는 api 추가
  - 예) Add API creating PDF
- `Remove` : 제거
  - 예) Remove unused local variables
- `Enhance` : 기능 또는 성능의 향상
  - 예) Enhance performance in select queries
- `Fix` : 버그, 오타, 스타일의 수정
  - 예) Fix typos in Javadoc
  - 예) Fix styles for standards of Naver Corp
- `Upgrade` : 라이브러리 버전 업그레이드
  - 예) Uprade commons-dbcp to 2.3.1
- `Document` : 문서화
- `Refactor` : 리팩토링
- `Update` : 다른 주변 상황에 맞추어서 갱신
  - 예) Update README.md for 1.0 release
- `Polish` : 잡다한 수정 묶음 (다른 적절한 문구가 없을 경우 사용)

#### Reference>

- [How to Write a Git Commit Message](https://item4.github.io/2016-11-01/How-to-Write-a-Git-Commit-Message)
