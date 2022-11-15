# git commit, git push 취소하기

## 1. 개요

작성된 커밋을 취소하는 방법과 `git push`로 깃허브 브랜치에 올려진 커밋을 취소하는 방법에 대해 살펴본다.

---

## 2. git commit 취소하기

커밋 메시지를 잘못 작성을 하였다면 아래의 명령어를 통해 작성된 커밋을 취소할 수 있다.

> $ git reset HEAD^

위의 경우는 가장 최근의 커밋을 취소하는 방법이다.

여러개의 commit를 취소하기 위해선 아래의 명령어를 작성하면 된다. 예는 마지막 2개의 commit를 취소하는 상황이다.

> $ git reset HEAD~2

---

## 3. git push 취소하기

`git push`로 인해 나의 코드가 깃허브 브랜치에 올려져 있을 땐 `git commit` 뿐 아니라 `git push` 까지 취소를 해야한다.

해당 경우는 매우 조심히 사용해야 한다. 혼자만의 프로젝트를 하는 경우는 그나마 괜찮겠지만 협업을 하는 상황에서는 돌이킬 수 없는 강을 건널 수도 있기 때문이다.

그 이유는 자신의 local의 내용을 remote에 강제로 덮어쓰기를 하기 때문이다. 즉, commit 이후의 모든 commit 정보가 사라지기 때문에 동료간의 상의 후 진행을 하는 것이 좋다.

git push 취소하기는 3가지의 과정으로 나뉜다.

---

### 3-1. commit 되돌리기

> $ git reset --soft HEAD^

또는

> $ git reset --hard HEAD^

`soft`와 `hard`는 아래의 특징을 가진다.

- soft: 내 로컬 디렉토리에 푸시한 데이터를 남긴다.
- hard: 내 로컬 디렉토리에 푸시한 데이터를 남기지 않는다.(완전히 되돌린다.)

---

### 3-2. commit 메시지 작성

되돌린 상태에서 코드를 수정 한 후 새로운 commit 메시지를 작성한다.

> $ git commit -m "feat: new commit"

---

### 3-3. 원격 저장소에 강제로 push 한다.

강제로 push... 왠만해선 실수로 돌리는 일이 없도록 하자!

> $ git push origin [branch name] -f

---

## 4. Conclusion

> 우테코를 진행하다 커밋 메시지를 잘못 작성되어 커밋을 취소한 적이 몇번 있었다. 커밋만 취소하는 경우엔 아직 깃허브 브랜치에 커밋이 올라가기 전이기 때문에 큰 부담이 없었지만, 푸쉬를 취소하는 경우는 깃허브 브랜치의 커밋을 강제로 바꾸는 것이기 때문에 두려운 마음이 있었다. 성공적으로 완료를 했지만, 혼자서든 협업에서든 다시는 실수를 하지 않도록 해야겠다. 항상 꼼꼼하게 살피고 커밋, 푸쉬를 하도록 하자.

---

## 참고

[[Git] git add 취소하기, git commit 취소하기, git push 취소하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)  
[[Git] git 푸시(push) 롤백(rollback)하기 강제로 되돌리기](https://hdhdeveloper.tistory.com/76)
