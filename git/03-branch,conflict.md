git brnach 활용하기, 충돌 관리하기
===============================

세미나 날짜 : 2013-10-03

branch가 필요한 이유
------------------

branch를 엄청난 속도로 만들고 통합할 수 있는 것이야말로 git의 최대 장점이라고 생각한다. 그러니 일단은 활용을 하는 게 좋겠다. 검색해 보면 작업 과정(Work Flow)에 따라 branch를 어떻게 활용하는지도 찾아볼 수 있다. 그러나 일단은 개인적, 기본적인 것부터 활용을 해 보자. 그래서 익숙해져야 Flow도 따라갈 수 있을 것 같다.

branch를 기본적으로 활용하는 것은 아주 쉽다. 다음 시나리오를 머리에 그려 보자.

첫 화면(index)을 대대적으로 개편하고 있는 상황이다. 지금 마무리 단계에 있어서 사흘 후면 런칭을 할 수 있다. 당연히 서비스되고 있는 첫 화면은 예전 화면이고, 지금 개편하고 있는 것은 로컬에서 작업하고 있다. 그런데 버그 신고가 들어왔다. 현재 서비스되고 있는 첫 화면의 메인 상품을 변경했더니 슬라이드가 다 깨진다! (왜 깨지는지 따지지는 말자. 그냥 버그다.)

어떻게 해야 할까? 세 가지 선택지가 있다.

1. 버전 관리 시스템을 사용하지 않는 경우 : 좀 위험하긴 하지만 FTP로 들어가서 온라인 디버깅을 한다.
1. 버전 관리 시스템을 사용하지만 branch를 사용하지 않는 경우 : 버전 관리 시스템에서 개편 작업 전으로 돌아가서 버그를 고친다. 개편 index에는 잡은 버그를 수동으로 반영한다.
1. 버전 관리 시스템을 사용하고, branch도 사용하는 경우 : master branch로 돌아가서 버그를 고친 뒤 개편작업 branch로 돌아온다. 개편작업 branch에서 `git rebase master`를 해 디버그한 코드를 branch에도 반영한다.

딱 봐도 세 번째가 가장 깔끔하다는 걸 알 수 있을 거다. 사실 버그가 간단히 잡히는 거고 개편작업도 규모가 크지 않은 거라면 1이나 2도 별로 문제가 안 될 수 있다. 하지만 개편의 규모가 크고 버그도 수정하는 데 꽤 많은 작업을 요하는 것이라면? branch는 필수적이다.

branch 연습으로 들어가기 전에 개념을 하나 익히자. branch 없이 작업한다는 말은, 하나의 branch에서 작업한다는 말로 이해하면 되겠다. branch를 만들지 않아도 우리는 master라는 branch에서 작업을 하고 있었던 거다. 새로운 branch를 만든다는 것은 master가 아닌 branch를 만든다는 것으로 이해하면 되겠다.

그럼 간단하게 branch 연습을 해 보자.

아래와 같은 코드가 있다.

    <ul class="unordered-list">
        <li class="list-item">이것은 1번째 li입니다.</li>
        <li class="list-item">이것은 2번째 li입니다.</li>
        <li class="list-item">이것은 3번째 li입니다.</li>
        <li class="list-item">이것은 4번째 li입니다.</li>
        <li class="list-item">이것은 5번째 li입니다.</li>
        <li class="list-item">이것은 6번째 li입니다.</li>
    </ul>

자, 인덱스를 개편해야 한다. branch를 만들자. branch 이름은 renewal이다.

    git branch renewal

branch를 만들었으면 이제 만든 branch로 옮겨 가 보자.

    git checkout renewal

이렇게 하면 아래와 같은 메시지가 나오면서 현재 branch를 알려 준다.

    Switched to branch 'renewal'

현재 branch가 어딘지 알려면 간단하게 `git status`라고 치면 된다. 첫 줄에 `# On branch renewal`라고 나올 거다.

인덱스를 개편하면서 목록 아래에 '더 보기' 버튼을 달았다. 그래서 코드는 아래처럼 됐다.

    <ul class="unordered-list">
        <li class="list-item">이것은 1번째 li입니다.</li>
        <li class="list-item">이것은 2번째 li입니다.</li>
        <li class="list-item">이것은 3번째 li입니다.</li>
        <li class="list-item">이것은 4번째 li입니다.</li>
        <li class="list-item">이것은 5번째 li입니다.</li>
        <li class="list-item">이것은 6번째 li입니다.</li>
    </ul>
    <a href="#">더 보기</a>

커밋도 하자.

어이쿠, 아직 개편이 다 끝나지 않았는데 원래 페이지 수정 요청이 들어왔다. `ul`위에 제목을 달아 달라는 거다. 평소때라면 당황했겠지만, 이제 branch를 사용하고 있으니 걱정할 거 없다. 메인 branch인 master branch로 돌아가자.

    git checkout master

이렇게 하면 master branch로 돌아올 수 있다. 아래와 같은 메시지를 만났다면 안심!

    Switched to branch 'master'

`git log`도 한 번 쳐 보자. 아까 renewal branch에서 커밋한 내용은 나오지 않을 것이다.

master branch에서 코드를 아래처럼 수정했다.

    <h2>Branch 연습용 목록</h2>
    <ul class="unordered-list">
        <li class="list-item">이것은 1번째 li입니다.</li>
        <li class="list-item">이것은 2번째 li입니다.</li>
        <li class="list-item">이것은 3번째 li입니다.</li>
        <li class="list-item">이것은 4번째 li입니다.</li>
        <li class="list-item">이것은 5번째 li입니다.</li>
        <li class="list-item">이것은 6번째 li입니다.</li>
    </ul>

이제 커밋을 하고, renewal branch로 돌아가자.

    git commit -am "제목 추가"
    git checkout renewal

그럼 다시 renewal branch로 돌아온 걸 확인할 수 있을 것이다. index.html을 열어 보면 master branch에서 작업한 `h2`는 없다는 걸 확인할 수 있을 거다.

그런데 개편작업이 끝나도 제목은 있어야 한다. 제목 하나 추가하는 건 어렵진 않은데, branch 연습중이니 git 기능을 이용하자. 그리고 지금이야 예제니까 간단하지 현실에선 이거보다 복잡한 일이 많이 벌어진다. 그럼 어떻게 해야 할까? master branch에 있는 걸 renewal branch로 가져오면 된다.

    git rebase master

그러면 아래와 같은 메시지가 나오면서 작업이 진행된다.

    First, rewinding head to replay your work on top of it...
	Applying: 더 보기 추가

이제 `git log`를 쳐 보자. 그러면 아래처럼 log를 확인할 수 있다.

    commit 26ac074b70e667222a51c882c96e2517aab66ba8
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:48:25 2013 +0900
    
        더 보기 추가
    
    commit 8d6fe1d6a6d4bd5dec10e46610f0eaf86df27825
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:48:38 2013 +0900
    
        제목 추가
    
    commit 61aa525f5fbbe1f671e45fa6b12a69c96b2f2e06
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:26:42 2013 +0900
    
        initial

master에서 작업한 '제목 추가'가 '더 보기 추가'보다 앞에 끼어든 것을 확인할 수 있다.

### rebase 개념

rebase는 단어 그대로 재구축이다. 근데 뭘 어떻게 재구축하는지 기억하는 게 중요하다. 현재 branch를 재구축하는 거다. 명령을 내릴 때는 뭘 기반으로 재구축하는 것은지를 명시해 주는 것이다. 그래서 `git rebase master`는 'master branch를 바탕으로 현재 branch를 재구축하라'는 명령이 된다.

다른 branch의 내용을 끌어오는 것으로는 `git merge master`도 있다. 개념은 똑같은데 '현재 branch에 master branch를 가져와서 합쳐라'라는 뜻이 된다. rebase가 과거 커밋들을 바탕으로 commit history를 재작성하는 반면에 merge는 합친 내용으로 현재 branch에 새로운 커밋을 하게 된다. rebase는 가져온 branch에 commit이 많았다면, 그만큼 과거의 commit이 많이 생기게 되지만, merge는 변경 내역을 가져와서 다 합친 다음 새로운 커밋을 한 번만 한다.

궁금하면 해 보시라. merge보다 rebase를 쓸 것을 권장하기 때문에 굳이 merge 예시를 해 보고 넘어가지는 않겠다.

충돌 관리
--------

앞선 예시를 바탕으로 충돌 관리 연습까지 계속 가 보자.

버전 관리 툴을 사용할 때 초보자들이 가장 무서워하고 잘 못 다루는 게 충돌(conflict)인 것 같다. 사실 별 거 아닌데 말이다. 같은 부분을 두 개발자가 동시에 고친 다음 한 명이 먼저 push를 하고, 다음 사람이 push를 하면 충돌이 일어난다.

혼자 일할 때도 충돌을 만날 수 있다. 우리가 연습할 게 이건데, branch에서 작업을 하다가 master에서 뭔가 고친 다음, branch에서 또 같은 줄의 것을 고치면 충돌이 일어난다. stash 상태에서 pull을 한 뒤 `git stash apply`를 했을 때도 충돌이 벌어질 수 있다. 정확힌 모르겠는데 같은 걸 동시에 고치지 않았는데도 충돌이 나는 경우도 있다. 여튼, git가 알아서 합치지 못하는 상황이 벌어지면 충돌이 벌어진다.

만능 해법이 있다. git가 시키는대로 하면 된다.

아까 예제에서 계속 가 보자. renewal branch에서 계속 작업할 거다. class명을 BEM 기반으로 사용하기로 결정해서 아래처럼 고쳤다.

    <h2>Branch 연습용 목록</h2>
    <ul class="index-list">
        <li class="index-list__item">이것은 1번째 li입니다.</li>
        <li class="index-list__item">이것은 2번째 li입니다.</li>
        <li class="index-list__item">이것은 3번째 li입니다.</li>
        <li class="index-list__item">이것은 4번째 li입니다.</li>
        <li class="index-list__item">이것은 5번째 li입니다.</li>
        <li class="index-list__item">이것은 6번째 li입니다.</li>
    </ul>
    <a href="#">더 보기</a>

이제 `git commit -am "BEM 방식 클래스명"`이라고 커밋을 해 준 뒤, `git checkout master` 명령으로 master branch로 돌아가자.

index.html을 아래처럼 고쳤다. 제목과 목록, 더 보기를 합쳐서 index-box라고 이름을 지어야 할 일이 생긴 거다. 아직 개편 전인데 말이다.

    <div class="list-box">
        <h2 class="list-box__title">Branch 연습용 목록</h2>
        <ul class="unordered-list">
            <li class="list-item">이것은 1번째 li입니다.</li>
            <li class="list-item">이것은 2번째 li입니다.</li>
            <li class="list-item">이것은 3번째 li입니다.</li>
            <li class="list-item">이것은 4번째 li입니다.</li>
            <li class="list-item">이것은 5번째 li입니다.</li>
            <li class="list-item">이것은 6번째 li입니다.</li>
        </ul>
        <a href="#">더 보기</a>
    </div>

`list-box`라는 클래스가 전체를 감쌌다. `h2`에도 클래스가 붙었다. `git diff`라고 쳐 보면 아래처럼 나온다. 맨 앞에 `-`가 붙은 게 삭제된 것, `+`가 붙은 게 추가된 것이다.

    diff --git a/index.html b/index.html
    index 49aefc6..29296ed 100644
    --- a/index.html
    +++ b/index.html
    @@ -5,15 +5,17 @@
         <title>Git Branch Example</title>
     </head>
     <body>
    -    <h2>Branch 연습용 목록</h2>
    -    <ul class="unordered-list">
    -        <li class="list-item">이것은 1번째 li입니다.</li>
    -        <li class="list-item">이것은 2번째 li입니다.</li>
    -        <li class="list-item">이것은 3번째 li입니다.</li>
    -        <li class="list-item">이것은 4번째 li입니다.</li>
    -        <li class="list-item">이것은 5번째 li입니다.</li>
    -        <li class="list-item">이것은 6번째 li입니다.</li>
    -    </ul>
    -    <a href="#">더 보기</a>
    +    <div class="list-box">
    +        <h2 class="list-box__title">Branch 연습용 목록</h2>
    +        <ul class="unordered-list">
    +            <li class="list-item">이것은 1번째 li입니다.</li>
    +            <li class="list-item">이것은 2번째 li입니다.</li>
    +            <li class="list-item">이것은 3번째 li입니다.</li>
    +            <li class="list-item">이것은 4번째 li입니다.</li>
    +            <li class="list-item">이것은 5번째 li입니다.</li>
    +            <li class="list-item">이것은 6번째 li입니다.</li>
    +        </ul>
    +        <a href="#">더 보기</a>
    +    </div>
     </body>
     </html>
    \ No newline at end of file

들여쓰기만 변경돼도 삭제된 것으로 처리되는 것을 알 수 있다. 아래 명령어로 커밋을 하고 renewal branch로 돌아가자.

    git commit -am "list-box 클래스 추가하고, h2에도 list-box__title 클래스 매김."
    git checkout renewal

master에서 변경된 사항을 끌어와야 하니 `gir rebase master` 명령을 내려 보자. 그러면 아래처럼 충돌이 발생한다.

    First, rewinding head to replay your work on top of it...
    Applying: BEM 방식 클래스명
    Using index info to reconstruct a base tree...
    M	index.html
    Falling back to patching base and 3-way merge...
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Failed to merge in the changes.
    Patch failed at 0001 BEM 방식 클래스명
    The copy of the patch that failed is found in:
       /path/to/.git/rebase-apply/patch
    
    When you have resolved this problem, run "git rebase --continue".
    If you prefer to skip this patch, run "git rebase --skip" instead.
    To check out the original branch and stop rebasing, run "git rebase --abort".

번역해 보면 이렇다.


    우선, 작업을 처음부터 재실행하기 위해서 head로 되감습니다.
    'BEM 방식 클래스명'을 적용중입니다.
    base tree를 재구축하기 위해 색인 정보를 사용중입니다.
    M index.html (index.html이 수정됐다는 뜻)
    base 재구축과 3방향 합치기가 잘못되는 경우에 대한 대비책을 만들고 있습니다.
    index.html을 자동으로 합칩니다.
    충돌 발생 (내용): index.html에서 합치기 충돌 발생
    '0001 BEM 방식 클래스명' patch 실패
    실패한 patch는 다음 경로에서 볼 수 있습니다.
        /path/to/.git/rebase-apply/patch
    
    이 문제를 수정한 뒤, "git rebase --continue"를 실행하세요.
    이 patch를 건너뛰려면, "git rebase --skip"을 실행하세요.
    rebase를 중단하고 원래대로 되돌리려면, "git rebase --abort"를 실행하세요.

사실 위쪽은 볼 거 없고 아래쪽 세 줄이 핵심이다. 고친 경우, 건너뛸 경우, rebase를 취소할 경우, 각각의 경우에 내려야 할 명령을 친절하게 알려 주고 있다. 메시지를 다시 보고 싶으면 `git status`를 하면 된다.

이제 명령도 알았으니 두려워할 거 없이 고치자.

index.html을 열어 보면 아래와 같이 충돌이 난 부분이 표시돼 있다.

    <<<<<<< HEAD
        <div class="list-box">
            <h2 class="list-box__title">Branch 연습용 목록</h2>
            <ul class="unordered-list">
                <li class="list-item">이것은 1번째 li입니다.</li>
                <li class="list-item">이것은 2번째 li입니다.</li>
                <li class="list-item">이것은 3번째 li입니다.</li>
                <li class="list-item">이것은 4번째 li입니다.</li>
                <li class="list-item">이것은 5번째 li입니다.</li>
                <li class="list-item">이것은 6번째 li입니다.</li>
            </ul>
            <a href="#">더 보기</a>
        </div>
    =======
        <h2>Branch 연습용 목록</h2>
        <ul class="index-list">
            <li class="index-list__item">이것은 1번째 li입니다.</li>
            <li class="index-list__item">이것은 2번째 li입니다.</li>
            <li class="index-list__item">이것은 3번째 li입니다.</li>
            <li class="index-list__item">이것은 4번째 li입니다.</li>
            <li class="index-list__item">이것은 5번째 li입니다.</li>
            <li class="index-list__item">이것은 6번째 li입니다.</li>
        </ul>
        <a href="#">더 보기</a>
    >>>>>>> BEM 방식 클래스명

뭣때문인지는 모르지만, 'HEAD'랑 'BEM 방식 클래스명'이랑 합치다가 git가 판단을 못 하고 멈춰선 거다. 우리는 사람이니까 명료하게 판단을 할 수 있다. 우리가 얻어야 할 코드는 위쪽의 `list-box`와 `list-box__title` 코드, 아래쪽의 `index-list`와 `index-list__item`이다. 두 코드를 통합해서 아래처럼 만들자.

    <div class="list-box">
        <h2 class="list-box__title">Branch 연습용 목록</h2>
        <ul class="index-list">
            <li class="index-list__item">이것은 1번째 li입니다.</li>
            <li class="index-list__item">이것은 2번째 li입니다.</li>
            <li class="index-list__item">이것은 3번째 li입니다.</li>
            <li class="index-list__item">이것은 4번째 li입니다.</li>
            <li class="index-list__item">이것은 5번째 li입니다.</li>
            <li class="index-list__item">이것은 6번째 li입니다.</li>
        </ul>
        <a href="#">더 보기</a>
    </div>

고쳤으니 저장을 하고, 아까 git가 시킨대로 명령을 내리자.

    git rebase --continue

이렇게 명령을 내리니 이상한 메시지가 또 나온다.

    index.html: needs merge
    You must edit all merge conflicts and then
    mark them as resolved using git add

해석하면 이렇다.

    index.html: 합칠 필요가 있습니다.
    모든 충돌을 해결하고 합쳐야 합니다. 그리고 나서
    git add를 이용해서 해결됐다고 표시하세요.

아놔 처음부터 말해 주지. 해결 표시를 해야 계속할 수 있단다. 시키는대로 하자.

    git add index.html

그리고 다시 `git rebase --continue` 명령을 내리자. 

    Applying: BEM 방식 클래스명

이렇게 깔끔하게 한 줄만 나오고 끝났다. `git log`로 로그가 어떻게 변했는지 보자.

    commit c10a1e433b75dfe937fe55b0a43530653c4918a8
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 02:50:45 2013 +0900

        BEM 방식 클래스명

    commit d2982daba131646e66591422c3ace21c9a64972e
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 02:54:18 2013 +0900

        list-box 클래스 추가하고, h2에도 list-box__title 클래스 매김.

    commit 26ac074b70e667222a51c882c96e2517aab66ba8
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:48:25 2013 +0900

        더 보기 추가

    commit 8d6fe1d6a6d4bd5dec10e46610f0eaf86df27825
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:48:38 2013 +0900

        제목 추가

    commit 61aa525f5fbbe1f671e45fa6b12a69c96b2f2e06
    Author: mytory <mytory@gmail.com>
    Date:   Thu Oct 3 01:26:42 2013 +0900

        initial

코드 수정 순서를 논리적으로 재배열했다.

    제목 추가 > 더 보기 추가 > list-box 클래스 추가 > BEM 방식 클래스명

순서로 코드 변경 기록을 재작성한 것이다.

간단한 충돌 관리를 해 봤는데, 핵심은 이거다. "git가 시키는대로 해라." pull을 했을 때도, stash apply를 했을 때도 마찬가지다.

### branch를 중앙 저장소에 보내기

branch를 중앙 저장소에 보내서 공동작업을 해야 할 때도 있을 거다. 원격에 저장을 하지 않으면 불안한 경우도 있을 테고 말이다. 그럴 땐 아래 명령을 사용한다.

    git push origin renewal

원격 브랜치명을 다르게 할 수도 있다. 물론 권장하지 않는다. 예외적인 경우에만 사용하게 될 거다.

    git push origin renewal:remote-renewal

branch를 master에 통합하고 삭제하기
----------------------------------

리뉴얼이 끝나서 renewal branch를 master branch에 통합하기로 했다. 그럴 땐 아래처럼 하면 되겠다.

    git checkout master
    git rebase renewal

아까는 renewal에서 master를 끌어왔지만, 이번엔 반대로 master에서 renewal을 끌어오는 거다. 그리고 아래 명령어로 renewal branch를 제거하면 된다.

    git branch -d renewal

### 팁 - stash 혹은 git reset HEAD~

branch에서 코드를 작성하다가 master branch에서 긴급하게 수정을 해야 할 게 생겼다! 그런데 아직 branch에서 작성중인 코드는 commit을 하기 싫다! 어떻게 해야 할까?

그럴 땐 두 가지 방법 중 하나를 택하면 된다.

    git stash

위 명령을 내리면 지금 작성중인 코드가 어딘가 다른 데로 저장되고 모든 코드가 원상태로 돌아온다. 원상태란 당연히 마지막 commit 상태를 말한다.

    git checkout master

이후 위 명령어를 쳐서 master branch로 이동한 뒤, 코드를 수정하고 커밋까지 한 다음 아래 명령어를 사용해 수정하던 코드를 되살린다.

    git checkout my-branch
    git stash apply

깔끔하게 완료!

다른 방법도 있다. 그냥 간단하게 커밋을 한 뒤 취소하는 거다. 

    git commit -am "임시"

코드를 수정하고 돌아와서 아래 명령으로 해당 branch의 마지막 커밋을 취소한다.

    git reset HEAD~

그러면 마지막 커밋이 취소되고 모두 수정중인 상태가 된다.

### 팁 - branch에서 작성중인 코드를 master로 옮기기

branch에서 한참 코드를 작성하다가, 이 코드는 branch에 있을 게 아니라 master에 있어야겠다는 생각이 들었다면? stash를 이용하면 된다.

    git stash
    git checkout master
    git stash apply

그럼 stash에 넣었던 코드가 master에 적용된다.

### 개념을 명확히!

master는 branch의 하나다. 따라서 위 예제의 master를 다른 branch로 바꿔 이해해도 모든 것이 제대로 돌아간다.

