# branch로 버전관리
### 1. branch 확인하기
f1.text 파일을 하나 만들고, add와 commit을 한다.  
여기서 commit 메시지는 ver.1로 한다.  
<br></br>

```bash
$ git branch
```
- 처음 init을 하면 master라는 기본 branch를 생성한다.
- \* master로 표시되게 되는데 이는 현재 사용중이라는 것을 나타낸다.
<br></br>

### 2. 새 branch 만들기
```bash
$ git branch exp
```
- exp라는 새로운 branch를 생성한 것
<br></br>

### 3. branch 전환하기
- master에서 exp 브랜치로 전환하기
```bash
$ git checkout exp
```
- checkout을 사용하여 브랜치를 전환할 수 있다.

### 4. branch 삭제하기
```bash
$ git branch -d 브랜치명
```
- 병합하지 않은 브랜치를 강제로 삭제할 댄 git branch -D를 사용한다.

### 5. branch 생성과 전환동시에 하기
```bash
$ git checkout -b exp
```
- git checkout -b 브랜치명 을 하면 브랜치 생성과 동시에 전환을 얻을 수 있다.

### 6. branch의 정보 확인
```bash
$ git log --branches --decorate
```
- 각각의 브랜치 내에서  git log를 하면 commit 히스토리를 확인할 수 있다.

```bash
$ git log --branches --decorate --graph
```
- 가독성을 더욱 높여주는 명령어 --graph이다.

### 7. 두 branch간의 비교
- 단순히 두 브랜치만을 비교하고 싶을 수 있다.
```bash
$ git log (브랜치1)..(브랜치2)
```
- 브랜치1에 없는 브랜치2의 커밋 내용을 보여준다.

### 8. branch 병합
- branch는 독자적인 버전 관리를 수행한다.
- 이 branch는 다시 하나의 branch로 병합이 가능하다.
- 병합을 하려면 대상이 두 개여야 하는데,git merge (브랜치명)은 브랜치를 하나만 입력받는다.
- 현재 사용하고 있는 브랜치가 병합되는 것이다.

```bash
$ git merge exp
```
- 현재 있는 master에서 exp를 병합

### 9. 충돌 해결
- 만약, 서로 협업하는 과정에서 branch를 각자 만들어 개발을 하면 소스코드를 합쳐서 테스트도 해봐야 하며 merge를 해야 한다.


> 1. 서로 다른 파일을 수정했을 때 -> 문제 없음
> 2. 서로 다른 파일을 수정했을 때 -> 다시 두 가지로 나뉨

> 2. 서로 같은 파일을 수정했지만 수정한 부분이 다를 때 -> 문제 없음  
> 2. 서로 같은 파일을 수정하고 수정한 부분이 겹칠 떄

- git은 자동으로 merge 할 수 없다고 사용자에게 알린다. 이를 충돌이라고 한다.