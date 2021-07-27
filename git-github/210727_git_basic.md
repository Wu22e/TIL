## Confilt(충돌) 상황

흔히 발생하는 문제 상황 confilct(충돌)를 만들어 보자 

main 브랜치의 hello.py 는

```python
print('world')
```

repeat-hello 브랜치를 새로 만들고 hello.py를 다음과 같이 

```python
for _ in range(10):
    print('hello')
```

이상황에서 main에 repeat-hello 의 hello.py를 합친다면?

![image](https://user-images.githubusercontent.com/52458039/127096548-6c688802-399e-4601-a0a2-7b4273617af8.png)

conflict 가 발생함.

main 브랜치에서 hello.py 파일을 열어보면

![image](https://user-images.githubusercontent.com/52458039/127096660-962717ab-9b75-4554-86f0-3d14a42cf792.png)

둘중 하나를 선택하거나, 얘네 둘을 재조합 하는 방법이 있다.

내가 만들지 않은 특수문자는 지우고 둘중 하나를 선택해서 지우면 된다.



재조합한다면 world라는 스트링을 repeate-hello에 적어서 반복문은 취한다고 한다면?

그냥 말그대로 코드를 그렇게 바꿔주고 저장하면  됨

![image](https://user-images.githubusercontent.com/52458039/127167534-80f5bc88-ebbf-4fdf-a378-ad5c2dd72fc0.png)

컨플릭트 파일이 있으므로 이러한 unmerged paths 라는 메시지가 뜸. merge 했을때 conflict 가발생했기 때문에 아직 merge가 되지 않은 상태이고, 충돌 상황에서 코드를 어떤방법을 적용할지 선택하여 소스코드만 수정된 상황이기 대문에 merge가 됬다고 commit을 날려야 함. (즉 위 상황이 오류 상황이 아님)

그러므로 다 재조합하거나 고쳣으니 

add 하고 commit 하면됨

![image](https://user-images.githubusercontent.com/52458039/127167224-3bff4dc7-ac76-4c1e-9349-c3a0f1139859.png)

머지 커밋 발생

![image](https://user-images.githubusercontent.com/52458039/127097195-92fe7475-e40c-4758-a0f6-dded1cbcaa69.png)

간단히  커밋 메시지 작성하면 됨

![image](https://user-images.githubusercontent.com/52458039/127097273-484607c3-4719-4d5b-9135-356d244fde88.png)

그럼 아래와 같이 MERGING과 merge branch 메시지가 뜸

> [참고]  vim 파일을 :q 또는 :wq로 나가는게 아니라 창을 그냥 거버리면 스왑파일이 생기게 되서 다시 빔 파일을 열면 Attention이 뜬다. 이때 그냥 q로 나오고 스왑파일을 지우면 됨

> 브랜치 삭제 : git branch -D 브랜치 이름

[참고] vim 명령어

```
ESC : 누른 상태가 명령 모드
:set nu : 라인 출력
i : insert 모드로 변경
: line number : 해당 라인으로 이동
:wq : 저장하고 종료(zz도 동일)
:q! : 강제 종료
dd : 줄 삭제
u : 되돌리기
yy : 라인 복사
p : 라인 복사후 붙여 넣기
```

> [참고] push한걸 되돌리려면 이전 버전으로 새롭게 push해야하거나 아예 삭제해야함 commit 한거는 뒤로 돌릴 수 있음. 그래서 push할땐 신중해야 함
>
> push는 왠만하면 안하는게 좋다. 올바르게 작동하는 코드를 확실히 올려야한다. 불완전한 코드를 올리면 협업하는 사람이 불완전한 코드를 받아서 작업하게 되므로 엄청난 문제가 생기게 된다.

> [참고] 코드리뷰 절차와 달리 실제 작업할 때는 브랜치 따서 작업후 main 브랜치로 와서 merge 하는게
> 실제 작업이다.
>
> 만약 코드리뷰면 브랜치 위에서 푸시하고 풀리퀘 보내면 됨

---

##### hexo를 이용하여 깃허브 블로그 만들기

1. 설치 : https://hexo.io/docs/setup
2. 테마 설정 : https://github.com/next-theme/hexo-theme-next#installation
3. 배포 설정 : https://hexo.io/docs/one-command-deployment#Git
4. 블로그 내용 설정 : https://hexo.io/docs/configuration

Next 테마 관련 설치 정리된 곳 : https://theme-next.js.org/docs/getting-started/installation.html

---

#### git flow 사용 하기

기본적으로 develop 브랜치에서 feature 브랜치를 따는 방식의 개발 방식이다.

이때 main 브랜치는 사용자가 사용하는 코드이므로 release 브랜치를 통해서 main 브랜치에 merge 하게 된다.

- 깃 저장소내 git-flow 사용 초기화

  ```
  git flow init
  ```

- 새 기능(feature) 시작 (feature/myfeature 브랜치 생성 됨)

  ```
  git flow feature start MYFEATURE
  ```

  이후 작업을 수행한 뒤 add, commit 해줌

- 기능 완료 (feature/myfeature 브랜치 삭제후 develop 브랜치로 merge 됨)

  ```
  git flow feature finish MYFEATURE
  ```

- 릴리즈 시작(release/v.x.x.x 브랜치 생성, v는 버전)

  ```
  git flow release start v1.0
  ```

- 릴리즈 완료(release/v.x.x.x 브랜치 삭제후 develop 브랜치와 main 브랜치에 코드 merge됨. 이때 main 브랜치에는 tag 메시지가 붙게 됨)

  ```
  git flow release finish v1.0
  ```

- 마지막으로 tag를 push 한다.

  ```
  git push --tags
  ```

git flow 전략을 시각화 잘 해놓은 사이트 : https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html

---

#### rename 하는 방법

일반적으로 이름 바꿀때는

```
mv README.md LICENSE
```

이런식으로 바꾼다. 하지만 이를 git status로 하면

![image](https://user-images.githubusercontent.com/52458039/127125491-954c4b55-4096-4835-b7ad-132d60cdf75f.png)

README.md는 본질적으로 레포에 대한 설명인데 , 즉 본질은 바뀌지 않고 이름만 바꼇을 뿐인데 deleted로, 원본이 삭제 됬다고 생각할 수 있다. 

다른 사람이 봤을때도 이 파일이 삭제되고 새로만들어졌구나 생각하게됨. 즉 연속성이 끊어지게 된다.

물론 다시

```
mv LICENSE README.md 
```

이렇게 되돌리면 git status로 하면 제대로 돌아오게 됨.

![image](https://user-images.githubusercontent.com/52458039/127125569-9807359d-2a95-449a-8ec5-233cc73f8f3a.png)

그럼 이거를 어떻게 해결할 수 있을까?

우선 해당 README.md 파일을 add 한뒤에

```
git mv README.md readme.md
```

이렇게 mv 앞에 git 키워드를 붙이게 되면 

git status를 보면 

![image](https://user-images.githubusercontent.com/52458039/127133208-d3e29473-6621-46d5-8845-86cc6208153d.png)

renamed 라는 메시지와 함께 연속성이 유지되게 된다.

그래서 레포를 쓸때, 이름을 바꾸거나 파일 이름을 옮기는것도 신중하게 해야함.

왜냐면 얘는 역사책과 같기 때문에 흐름이 다 기록이 되기 때문이다.

---

#### undo 하기

readme.md 빈파일에 내용을 적고 git status를 누르면

![image](https://user-images.githubusercontent.com/52458039/127134232-e21a10aa-34ed-41b5-b07a-46be0423cc88.png)

당연히 modified가 뜬다. 빈파일에 내용을 적었으니까.

readme plz 라고 썻는데 공식적이지가 않아서 바꾸고 싶다면?

```
git checkout -- readme.md
```

를 치고 git status 하면 

![image](https://user-images.githubusercontent.com/52458039/127159184-5900cb39-0a11-416a-b192-fe5749f9463b.png)

변경사항이 없고

cat readme.md 하면 아무 내용이 없다. 즉 이전 버전으로 돌아오게 됨

checkout 대신 아래와 같이 restore를 사용해도 됨.

```
git resotre readme.md
```

사실 파일 한두줄 지우는건 그냥 지우는게 낫지만 돌이킬수 없을 정도로 많이 변경했다면 checkout 혹은 restore를 사용하면 가장 최신 이전 버전으로 돌아감.

---

#### unstage 사용하기

git 로컬에 touch world.py를 만들게 되면 blob에 변경사항이 생긴다. blob상에 새로운 world.py가 추가됬다.

그래서 나는 아무생각없이 관습적으로 git add world.py를 했음. 근데 아무리 생각해도 빈파일을 올리는건 아닌것 같다고 생각하여 다시 add를 취소 하고 싶다면?

그럼 blob에 올라간걸 다시 내려가게 해야하는데 그것을 언스테이징이라고 하고 그 키워드가 reset이다.

일단 git status 누르면

![image](https://user-images.githubusercontent.com/52458039/127159840-e0b0801d-d4a4-4d66-af4a-8c55ef114316.png)

new file이 뜨고 커밋을 기다린다고 한다

이때 git reset HEAD world.py를 해서 내려주고 (HEAD는 최신 브랜치를 뜻함, 특정 브랜치가 아니라 무조건 최신 브랜치, 위치에서 내려주겠다는 뜻임)

git status를 하면

![image](https://user-images.githubusercontent.com/52458039/127159958-aa1534c0-2e60-46de-847d-33494554c841.png)

다시 untrack이 된다.

---

#### 커밋 메시지를 잘못 작성했다면?

위에서 만든 world.py 파일을 add commit 해보자.

git commit -m "feat: Create hell.java "

이렇게 커밋을 찍었다면? 

메시지를 잘못작성했으므로 커밋을 수정해야함. (가정: 파이썬 파일인데 자바 파일이라고 잘못 작성했 다면)

커밋 수정시에는 

```
git commit --amend 
```

amend 키워드를 쓰면 최신의 커밋을 바로 수정할 수 있다.

![image](https://user-images.githubusercontent.com/52458039/127160431-fac9b329-c33f-4b52-ad9e-bf54af93fdc8.png)

![image](https://user-images.githubusercontent.com/52458039/127160500-ecd9a4d7-c3d5-4bdb-88db-cc7816abc435.png)

그럼 다시 commit 메시지를 작성할 vim 파일이 켜진다. 다시 수정하면 됨

근데 시점이 한시점이라도 지난걸 수정하려면 rebase를 사용해야 함.

---

#### revert 사용하기

기록이 만약 잘못 되어있다면?

> 여기서 사람마다 생각이 다른데, 모든 activity가 그 시점에 잘 남아있어야 한다고 주장하는 사람이 있고, 개발했던 기록만 잘 남아 있는게 좋다라고 생각하는 사람이 있다.
>
> 근데 어떻게든 역사를 기록하는 입장에서 잘못 기록한것이 문제로 생각할 수 있다. 내가 잘못한 것도 잘못한대로 기록을 남겨야한다.

한가지 방법으로는 reset과 강제 푸시 하는 방법이있다.

기록을 지우기 위해 reset을 쓰는데

특히 hard reset을 쓰면  되돌릴 수 없다. 또한 git push -f 를 하게 되면 (force) 강제로 푸시가 됨. 이렇게 되면 로컬 에 있는 내용이 전부 강제 푸시가 되므로 협업할때 굉장히 많은 문제를 초래함 

겉보기에는 가장 깔끔한 해결책으로 보이나 팀원들과 공유하는 브랜치이고, 내가 커밋들을 되돌리기 전에 다른 팀원이 혹시나 내가 작성한 커밋들을 이미 pull로 땡겨 갔다면, 그때부터 다른 팀원의 로컬 저장소에는 내가 되돌린 커밋들이 남아있게된다. 그렇게 되면 그 커밋들이 되돌려 진줄도 모르고 팀원은 자신이 작업한 커밋과 함께 push할 것이고 결국 다시 원격 저장소에 내가 되돌렸던 커밋들이 추가되게 된다.

이 방식은 나 혼자만의 브랜치에서 커밋 push 하고 그걸 되돌리고 싶은경우나 팀원들과 소통을 하여 내가 되돌린 커밋을 pull 해간 팀원이 없다고 확인된 경우에 안전하게 사용할 수 있다.

어쨋든 팀원과 협업할때는 reset 방식은 추천하지 않고, 잘못된 이력도 남겨 놔야한다. 잘못된 이력또한 팀원과 공유하게 되므로 위와 같은 문제가 발생하지 않는다.

이걸 바로 revert라고 한다. 

(reset은 최대한 안 쓰는게 좋다.)

- revert

g1.md, g2.md, g3.md 파일을 만들어서 커밋해 보자.

![image](https://user-images.githubusercontent.com/52458039/127161217-318a0170-b9d6-4b55-a1a6-7ac4c121ae62.png)

그럼 지금 g1.md g2.md g3.md가 있을 거고

git log를 보면 

![image](https://user-images.githubusercontent.com/52458039/127161660-27aa180d-2d3e-4b14-ad1f-8b4551b2ad24.png)

g1, g2, g3 만든 내역이 커밋이 찍힘

이상황에서 하드 리셋을 하면 커밋 3개 찍힌 로그를 삭제하고 g1,g2,g3.md 파일을 삭제할 수 있다.

하지만 이방식은 매 과정마다 실수할 가능성이 높기에 선호 하지 않는다.

(왜냐면 과거 이력이 깔끔하게 사라지게 되어 commit log tracking이 힘들어 지게 된다.)

이땐 차라리 revert를 쓴다. (잘못한 이력도 commit으로 박제하고 수정한 이력을 남기기 위함이다)

일단 세 시점 뒤로 돌아가야 한다.

```
git revert --no-commit HEAD~3.. (또는 git revert [되돌리고 싶은 commit의 hash] 로도 revert 가능)
```

이렇게 하면 최신시점 HEAD로 부터 3.. 즉 3번째 뒤로 돌아가게 된다. (여기서 --no-commit을 쓰는 이유는 revert역시 특정 커밋에서의 변경 사항을 제거하는 또다른 커밋을 생성하는 명령어 이므로 되돌리고 싶은 커밋이 많아지면 그만큼 revert 커밋이 생겨나게 되므로 이때 --no-commit 옵션을 이용하면 revert를 위한 커밋을 하나만 생성할 수 있다.)

![image](https://user-images.githubusercontent.com/52458039/127163951-129b0092-2cbd-4ea0-bf25-1256620f75fe.png)

그리고 ls 하면 g1.md g2.md g3.md가 사라짐

그리고 git status하면

삭제한 내역을 커밋하게 된다.

> 이때 commit hash를 git log로 확인하면 3시점 뒤로 온것을 확인할 수 있다. 

그리고 다시 git commit 하면
![image](https://user-images.githubusercontent.com/52458039/127165066-5533e0cf-fee6-4feb-bdf6-89087547ce78.png)

얘는 rever commit 이므로 vim 파일로 어디로 돌아갔는지 커밋 내역이 뜨고 거기에 내 나름대로 왜 돌아갔는지 사유를 쓰면 된다. 

```
아 제가 파일을 잘못 만들었습니다.
임직원을 대표하여 사죄드립니다.
```

뭐 이런식으로 말이다. 그리고 commit 날리면 

![image](https://user-images.githubusercontent.com/52458039/127165307-ba5215b7-2b52-4f88-95db-df977deb427f.png)

이전에 잘못 올렸던 log 3개 내역은 남아있다. 하지만 g1.md g2.md g3.md파일은 삭제되게 된다.

이걸 남겨놓는게 깔끔하고 좋다.

이렇게 하면 deleted한 내역이 커밋되기 떄문에 이 레포를 가져간 (풀받은) 모든 파일에 g1 g2 g3 md 파일이 없게 된다. 

특히 협업할때 이걸 잘 써야 한다.

---

#### 협업은 어떻게 이루어 지는가? (밑의 글을 쭉 읽고 동료와 실습 해보자.)



> [참고] 보통 레포에 해당되는 모든 이슈들은 Issues에서 글을 쓰면 됨. (보통 버그내용 들을 정리함)
>
> 오픈 소스 프로젝트를 할때는 이슈 트래킹할때 영어로 써야 많은 사람들이 볼 수 있다.
>
> Projects 에는 프로젝트 템플릿으로 칸반을 쓸 수 있다.
>
> todo -> in progress -> done 이런식의 칸반보드(스크럼 보드) 가 있고,
>
> 여기다가 issue 카드들을 옮겨놓을 수 있다.
>
> 참고로 이슈들을 풀리퀘해서 받아들여지면 Done으로 옮겨지게 됨.

**협업 절차 :**

콜라보레이터 말고

한사람의 레포를 포크를 해서 코드를 완성해 나갈 수 있다.

우리가 코드리뷰를 하기위해 콜라보레이터로 초청해서 일을 할 수 있었는데, 실제로 콜라보레이터를 추가하면 

콜라보레이터로 추가된 사람이 다른사람의 레포에서 마치 내것인것 처럼 작업할 수 있음.

그렇게 되면 동선이 꼬이게 된다. 

모든 협업 기능들은 싸우지 않기위해 만들어진 기능이다.

그래서 콜라보레이터는 코드리뷰어를 추가한다거나 이슈, 프로젝트 위키처럼, 부가기능들을 관리해줄 사람들에게 부여하는 편이다. 

그럼 실제로 어떻게 협업이 이루어지냐면 forking workflow로 이루어짐

우선 팀장이 레포를 만든다 레포 이름 : `helloworld`

readme.md만들고 바로 만든다.

그럼 헬로월드 레포에 리드미 엠디가 생김

그리고 팀장이 자기 로컬에 클론한다.

그럼 헬로월드 레포가 클론된다. 그럼 

forking workflow에서 gitflow를 적용해서 협업한다.

우선 클론을 햇기때문에 git status를 찍으면 main 브랜치와 새로운 레포가 만들어 진것을확인할 수 있다.

그리고 git flow init을 하고

git branch로 확인하고 ls로 확인하고 

그럼 readme.md만있다.

그럼 이제 사용자들이 와서작업할 새 파일을 작성한다.

일단 touch hello.py 만들고

git add, git commit -m "feat: creat~~" 까지한다.

그리고 이상태에서 git branch 하면 develop 브랜치에서 push 하면됨 (main은 팀장이 관리함)

develop은 첫 푸시니까 -u로 git push -u origin develop 한다.

이럼 push가 완료되고, 팀장은 develop이란 브랜치와 대상파일(hello.py)을 확인한다.

그리고 이 주소를 팀원한테 알려주면 된다.



그럼 이 주소를 받은 팀원은 이동해서 팀장ㅇ님이 이런일을 한걸 확인하고 그리고 issue를 쓰러간다.

`helloworld 기능 구현` 이란 제목으로

내용은

```
- [] 3의 배수일때 hello 출력
- [] 5의 배수일때 world 출력
- [] 15의 배수일때 helloworld 출력
- [] 나머지 모든 경우, 숫자 출력
```

여기서 - [] 는 체크박스로 표시할 수 있다. 그래서 이슈쓸떄는 기능구현을 뚜렷하게 리콰이어먼트가 정해져있다면 체크박스로 만들고 내가 어느정도 하고있는지 명확하게 알려주는게 중요하다.



계속 팀원입장에서 일해보면, 이슈쓰고 위에 fork라는 버튼을 누른다. 그럼 지금은 팀장님의 helloworld 레폰데, 

팀원의 레포가 된다. 주소를 봐도 팀원의 helloworld가 된다. 그대로 그 상태로 다시 git clone에서 작업한다.

그리고 git flow init하면 먼저 해준다.

우리가 해야할 일은 hello world를 구현해야함

근데 develop에서 바로하면 안되니까 

```
git flow feature start hw-for-if
```

라고 피쳐브랜치를 따고 거기서 작업한다



hello.py를 다음과 같이 변경

```python
for i in range(1, 16+1):
    print(i)
```

git status -> git add. git commit

여기서 

feat: print numbers ~~ 커밋메시지 쓰고

그럼 1부터 16까지 출력 되는것 봤고, 다시  hello.py를 고친다.

```python
for i in range(1, 16+1):
    if i%3 == 0:
        print('hello')
    else:
        print(i)
```

이렇게 하고 다시 git status -> git add, git commit

feat: print 'hello' if i is times of 3 or, print numbers for other cases 

라는 커밋메시지로 올린다.

그리고 팀장 레포로가서 이슈에다가 체크박스만든걸 체크해준다. 

그리고 또 다시 남은 구현사항을 구현한다.

```python
for i in range(1, 16+1):
    if i%3 == 0:
        print('hello')
    elif i%5==0:
        print('world')
    else:
        print(i)
```

확인해보고 git status git add git commit 한다

feat: print 'world' if i is times of 5 라고 커밋메시지 올리고

또 팀장레포에서 체크

또 마지막 남은사항 구현

```python
for i in range(1, 16+1):
    if i%15 ==0:
        print('helloworld')
    elif i%3 == 0:
        print('hello')
    elif i%5==0:
        print('world')
    else:
        print(i)
```

마지막 구현하고 git status git add git commit

그리고 마지막 커밋 메시지는 다음과 같이 쓴다.

```
feat: print 'helloworld' if i is times of 15.

resolved: #1
```

이렇게하고 팀장님 레포에 이슈를 만들고, 그리고 이슈는 모두 넘버링된다.

그리고 이 1번이슈에 대한일을 해왔으니, 1번이슈 처리가 끝났다는 뜻으로 

마지막 커밋에다가 `resolved: #1` 를 적어 줘야 한다.



그리고 이 브랜치에 푸시할 필요없이

git flow feature finish hw-for-if 해주면

develop으로 머지됨.

이상태를 push 한다.

내쪽도 첫푸쉬니까 -u

git push -u origin develop 이렇게 하면

내 레포로 갔을때 compare & pull request가 뜨고

이걸 누르고

원격저장소에 가서

상태를 보면 화살표 방향으로 

팀원/helloworld의 develop에서 팀장/helloworld의 develop으로 가는거다.



풀리퀘 할때는

제목 : print helloworld task done

내용 : close: #1



여기서 해쉬태그 넘버는 이슈의 넘버를 의미함.

이렇게 하고 풀리퀘하면 내가 끊어서 커밋한 내역을 모아서 풀리퀘를 생성함.

그럼 팀장은 메일을 받게되고 그메일의 링크를 가면 풀리퀘 링크로 가게됨.



그럼 팀장은 코드리뷰를하게된다.

그럼 팀장은 뭐 한줄로 할수없냐? 는식의 코드리뷰와 request change를 보내준다.

그럼 팀원은 그걸 보고 코멘트로 저번에 시킨거아닙니까? 하고 다시 작업하면 됨.



이렇게 pull request열려있는 상태에서 다시

hello.py를 열어서 (develop 브랜치임)

```python
for i in range(1, 17):
    if i%3 ==0 or i%5 ==0:
        print('hello'*(i%3==0) + 'world'*(i%5==0))
    else:
        print(i)
```

이렇게 하고 다시 git add hello.py , git commit 

feat: addtional request done



rpint helloworld one line

이렇게 메시지 올리고

다시 push origin develop 푸시하면 

풀리퀘에서 코드리뷰했던 부분이 내가 작업했던 한라인으로 바꾼 코드로 반영이 된다.



그럼 팀장이 resolve처리하고 confirm merge하면 끝난다.



이렇게 풀리퀘가 머지되면 

그럼 나머지 팀원들은 여기서 이 내역들(develop) 을 업데이트 해야한다.



팀장 입장에선 git fetch origin develop이고

팀원 입장에서는 git upstream origin develop 한다.

그리고 git merge FETCH_HEAD 하면 끝남.



이게 2명이서 핑퐁하면 쉬워보이는데, 3명이상이 되면 복잡해진다.