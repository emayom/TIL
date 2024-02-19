# git stash로 잠깐 치워두기
> git 커맨드를 잘 활용하면 작업의 효율과 편의성을 높일 수 있다.  
> 협업이 아니더라도 정말 유용하게 사용한 명령어 `git stash`  
> 200% 활용하기 위해 톺아보자! :wave:

##### Table of Contents
- [git stash란?](#git-stash란)  
- [git stash 사용하기](#git-stash-사용하기)
    - [stash 생성](#stash-생성)
    - [stash 목록 확인](#stash-목록-확인)
    - [stash 내용 확인](#stash-내용-확인)
    - [stash 적용](#stash-적용)
    - [stash 삭제](#stash-삭제)
    - [stash 적용과 삭제를 동시에](#stash-적용과-삭제를-동시에)
    - [stash 적용 브랜치 생성](#stash-적용-브랜치-생성)
- [오늘의 결론](#오늘의-결론)
<br>

### git stash란?
`git stash`는 stash라는 단어 그대로 <u>숨기고 싶은 작업이 있을 때</u> 사용할 수 있는 git 명령어 입니다. 

- **stash 언제 사용할까?**  

    <mark style="background: linear-gradient(to top, #FFE699 30%, transparent 20%);">현재 브랜치에서 아직 커밋하지 않은 변경 사항이 있지만, 다른 브랜치로 변경하거나 다른 작업을 수행해야 할 때</mark> 사용할 수 있습니다. 작업 중인 변경 사항을 커밋하지 않았을 경우 브랜치를 이동 시 충돌 가능성이 존재하는데 이때 `git stash` 커맨드를 통해 변경 사항을 스택에 임시 저장하여 워킹 디렉토리를 깨끗하게 정리할 수 있습니다.  

- **stash 어떤 옵션이 있을까?**    

    stash 하위에는 `list`, `show`, `drop`, `pop`, `apply`, `branch`, `push`, `clear`, `create`, `store` 같은 변경 내용들을 저장하고, 관리 및 복원하는데 도움이되는 기능들이 존재합니다.  
    
    아래에서 하나씩 확인 해 볼테지만 보다 자세한 내용은 터미널에 `git stash --help` 커맨드를 입력하면 자세히 확인 해 볼 수 있습니다! :point_down:

    ```shell
    git stash --help 
    ```

<br>

### git stash 사용하기
#### stash 생성
```shell
git stash

# staging area의 파일을 stash 제외한다. (staging area 영역 유지)
git stash --keep-indx

# 메시지와 함께 stash한다. 
git stash -m 
git stash --message <message> 
```
> 이외에도 `git stash save`, `git stash push` 커맨드를 활용하여 생성할 수 있습니다.  
> `save` 커맨드는 동일한 작업을 수행하는 대체 될 가능성이 있으니 `push` 커맨드를 사용하는 것을 권장합니다!
```shell
git stash save 
# 메시지와 함께 stash한다. 
git stash save <message> 

git stash push 
# 메시지와 함께 stash한다. 
git stash push -m <message> 
```

> :exclamation: 기본적으로 `git stash`는 추적 중인 파일만 저장합니다.  
```shell
# 추적 중이지 않은 파일도 포함하여 저장한다. 
git stash -u
git stash --include-untracked
```

> stash를 활용하면 `git clean` 커맨드 대신 안전하게 워킹 디렉토리의 파일들을 치워버릴 수도 있다. 
```shell
# 모든 파일을 stash한다. 
git shash -a
git stash -all
```

> `--patch` 옵션을 사용하면 대화형 프롬프트를 통해 저장할 것과 저장하지 않을 것을 지정할 수 있습니다.
```shell
git stash -p
git stash --patch
```

> stash 작업 중 터미널에 출력되는 메시지를 최소화 할 수 있습니다.  
```shell
git stash -q 
git stash --quit
```
<br>

#### stash 목록 확인
```shell
git stash list 
```
<br>

#### stash 내용 확인 
> `--color`, `--no-color` 혹은 `--stat` 등등 다양한 `diff-options`를 설정할 수 있다.  
> https://git-scm.com/docs/diff-options
```shell
git stash show  

# 변경 내용을 자세하게 보여준다. 
git stash show -p 
```
<br>

#### stash 적용 
```shell
# 가장 최근의 stash를 적용한다.
git stash apply

# 지정한 이름의 stash를 적용한다.
git stash apply stash@{2}
```

> :exclamation: git은 stash를 적용할 때 staged 상태였던 파일을 자동으로 다시 staged로 만들어주지 않는다.
```shell
git stash apply --index
```
<br>

#### stash 삭제
> :exclamation: `apply` 옵션은 단순히 stash를 적용하는 것 뿐, stash는 여전히 스택에 남아있다.   
> `git stash drop` 명령어를 사용하여 해당 stash를 제거해야 한다.
```shell
git stash drop

# stash name을 지정해 적용한다. 
git stash drop stash@{0}

# 모든 stash를 삭제한다.
git stash clear 
```
<br>

#### stash 적용과 삭제를 동시에
> stash를 같은 브랜치에 적용하거나 여러 브랜치 간에 공유가 필요 없을 경우 `git stash pop` 커맨드를 사용할 수 있다.   
> `apply`와 `drop`을 동시에 적용하기 때문에 저장한 내용이 더이상 필요하지 않을 경우에만 사용해야 한다. 
```shell
git stash pop 
```
<br>

#### stash 적용 브랜치 생성 
> stash 시 저장된 변경 사항을 기반으로 한 새로운 브랜치를 간편하게 생성할 수 있다.  
> 새로운 기능을 위한 브랜치를 생성하거나 버그 수정을 위한 브랜치를 생성할 때 유용하다! 
```shell
git stash branch <branch name>
```
<br>

### 오늘의 결론
`git stash` 커맨드에도 다양한 옵션이 존재하니 정확하게 숙지하고 상황에 따라 올바른 옵션들을 사용하는게 좋을 것 같습니다!  
`git stash`가 임시 저장이라는 매우 유용한 커맨드이지만 커밋을 대체하여 잦은 남용이나 장기적으로 변경 내용을 저장하는 목적으로 사용하지 않는 것이 좋습니다.  

메시지를 지정하지 않고 누적하여 stash를 사용하였더니 drop되지 않은 stash들을 시간이 지나니 뭐가 뭔지 알아 볼 수 없던 적이 있었습니다.
주기적으로 stash 목록을 정리하고 되도록 의미있는 이름을 지정해서 사용하는 것을 추천드립니다! 
<br>
