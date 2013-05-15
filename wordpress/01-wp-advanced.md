# 워드프레스 고급 코딩하기

날짜: 2013-05-13   
사용한 워드프레스 버전 : 3.5.1

## 개요

이 글은 워드프레스로 기본적인 작업을 할 수 있는 **개발자**를 대상으로 한다. 따라서 기초적인 설명은 하지 않는다.

워드프레스로 모든 웹사이트를 만들 수 있다. 아무리 복잡한 것이라도 말이다. 물론, 워드프레스를 사용하는 게 가장 효과적인가는 차치하고 말이다.

워드프레스는 기본적으로 글(post)과 페이지(page)를 제공한다. (워드프레스에서 사용하는 용어는 한 번만 한글과 영어를 병기한 뒤 이하는 모두 영어로 쓰겠다. 어차피 개발자들이 볼 테니 익숙해지기에 그게 더 나을 거다.) 

post의 분류(taxonomy)에는 카테고리(category)와 태그(tag)가 있다. 

page는 기본 taxonomy가 없다. 그러나 계층형(hierarchical)으로 만들 수 있다. 부모 페이지와 자식 페이지를 설정할 수 있는 거다.

### 새로운 콘텐츠 타입

개인 블로그나 간단한 웹사이트는 post와 page 타입과 기본으로 제공하는 category, tag taxonomy만으로 충분히 소화 가능하다. 그러나 출판사의 책 판매 웹사이트를 만들어야 한다면? 이야기가 달라진다. 물론, 사용자 필드(custom field)를 이용해서 정보들을 넣는 선택을 할 수도 있다. 하지만 깔끔한 구조는 아니다. post나 page가 아닌 book이라는 새로운 콘텐츠 타입을 만드는 게 가장 깔끔할 것이다. 이렇게 새로운 콘텐츠 타입을 custom post type이라고 한다. 개발자가 작업을 하면 관리자 페이지로 이런 새로운 콘텐츠 타입을 쉽게 통합할 수 있다. 이 부분의 기본적인 내용은 [Codex의 register_post_type 함수 레퍼런스](http://codex.wordpress.org/Function_Reference/register_post_type)를 참고하면 된다.

book에는 저자와 주제라는 taxonomy가 필요할 거다. 저자는 한 책에 여럿이 붙을 수 있다는 점에서 tag와 성격이 비슷한 taxonomy고 주제는 계층구조라는 점에서 category와 유사하다. 물론 세부적인 차이는 있지만 말이다. 여튼간에 이런 경우 custom taxonomy라는 기능을 이용하면 된다. 이 부분의 기본적인 내용은 [Codex의 register_taxonomy 함수 레퍼런스](http://codex.wordpress.org/Function_Reference/register_taxonomy)를 참고하면 된다.

### 관리자 페이지 코딩하기

book이라는 custom post type을 만들었다고 끝나는 게 아니다. book에는 post보다 더 많은 정보가 들어간다. 목차, 미리 보기, 부제 같은 것들 말이다. 이런 것들을 어떻게 구현할 지는 전적으로 개발자의 기획에 달린 문제다. 예시로 내 기획을 말해 보자면, 나는 목차와 미리 보기는 custom field를 이용하고, 부제는 제목을 파싱하는 방법을 이용하겠다. 

부제는 책 제목의 일부고, 웹브라우저의 title 영역에 노출되야 하기 때문이다. custom field로 한다고 웹브라우저의 title 영역에 노출 못 할 건 아니지만 손대야 하는 데가 너무 많고, 잦을 수 있다. 그러니 속편하게 제목(post_title)에 부제까지 포함해서 넣고 페이지에서 보여 줄 때만 파싱해서 모양만 다르게 보여 주는 게 나을 것이다.

부제는 파싱해야 하긴 하지만, 관리자 페이지에서 손댈 건 없어 오히려 간편하다. 그러나 목차와 미리 보기는 book 입력/수정 페이지에서 에디터를 제공해야 할 것이다. 이러면 개발자가 손을 대야 한다. 이건 메타 박스(meta box)를 등록해서 해결하면 된다. post 편집(입력/수정) 페이지를 구성하는 각각의 한 요소를 워드프레스에서는 meta box라고 부르나 보다. meta box를 추가하려면 [Codex의 add_meta_box 함수 레퍼런스](http://codex.wordpress.org/Function_Reference/add_meta_box)를 참고하면 된다.

관리자의 목록 화면에서 custom field도 표시해 주면 좋을 것이고, custom field나 taxonomy를 바탕으로 검색도 할 수 있다면 좋을 것이다. 이런 것들도 모두 처리할 수 있도록 해야 할 것이다. Custom Taxonomy로 필터링하는 것은 [[워드프레스] 관리자 글목록에서 custom taxonomy로 필터링할 수 있도록 하기](http://mytory.net/archives/9090)를 참고하면 되고, Custom Field를 바탕으로 검색하도록 하는 것은 $wp_query를 이용해서 해야 하는데, 현재 따로 참고할 만한 게 없으므로 내가 설명할 것이다.

### 새로운 콘텐츠 타입을 클라이언트 단에서 불러 오기

book을 만들었으니, 그걸 클라이언트 단에서 목록으로 제공을 해야 할 것이다. 저자나 주제별 목록도 제공해야 할 거고 말이다. book 상세 보기 페이지도 제공해야 할 것이다. 모두 간단하다. page나 post를 슬러그(slug)랑 엮어서 파일명을 지정하면 해당 파일을 바탕으로 보여 주는 기능을 알고 있을지 모르겠다.

예컨대, 어떤 page의 slug를 'sign-up'으로 지정했다고 하자. 이 sign-up은 알겠지만, 회원 가입 페이지고, 에디터로 작성하는 건 한계가 있다. 그래서 직접 하드코딩한 화면으로 보여 주기로 결정했다. 어떻게 해야 할까? 테마 폴더에 `page-sign-up.php`를 만들고 직접 코딩을 하면 된다. 워드프레스는 'sing-up' page가 호출되면 테마 폴더에서 일단 page-sign-up.php가 있는지 알아서 살펴 보고 없으면 DB에 있는 걸 바탕으로 화면을 구성한다. 파일이 있으면 DB를 찾지 않고 바로 해당 파일을 뿌려 준다. (slug가 아니라 ID값으로 할 수도 있다. 그러나 ID로 하면 파일명으로 내용을 추측할 수 없게 되니 ID로 파일을 만들지는 말자.)

(페이지와 테마의 파일이 어떻게 대응하는지 궁금하다면 [[워드프레스] 테마에서 템플릿 파일 매칭 순서](http://mytory.net/archives/10119)나 [Wordpress.org의 Template Hierarchy 그래프](http://codex.wordpress.org/images/1/18/Template_Hierarchy.png)를 참고하라.)

차근차근 살펴 보자. 일단 book의 목록은 기본적으로 `archive-book.php`에 의존해서 로딩되게 된다. `archive-book.php` 파일이 없다면 `archive.php` 파일에 의존해서, `archive.php` 파일도 없다면 `index.php`에 의존해서 로딩될 것이다. 책은 보통 표지와 제목, 필자, 쪽수 등등의 정보를 목록에서 모두 보여 주므로 `archive-book.php` 파일을 만들어서 작업하는 편이 좋을 것이다.

비슷하게, 저자와 주제별 목록을 보여 주려면 `taxonomy-author.php` 파일과 `taxonomy-subject.php` 파일을 만들고 작업을 하면 된다. (저자의 slug가 author고 주제의 slug가 subject라고 가정한 것이다.) 그러나 특별히 저자별이나 출판사별 목록이 책 목록과 차별화되는 내용이 없으니 굳이 파일을 만들지 않아도 될 것 같다. 그러면 아마도 워드프레스가 알아서 `archive-book.php` 파일을 이용해 표시할 것이다. 만약 워드프레스가 자동으로 그렇게 하지 않는다면 `taxonomy.php` 파일을 만들고 `include "archive-book.php";`라고만 적어 주면 그만이다.

### 개요를 나가며

워드프레스를 이용해서 복잡한 웹사이트를 온전히 만드는 데 필요한 과정을 요약적으로 설명했다. 워드프레스에서 사용하는 영어 용어를 그대로 사용했으니, 검색에 능한 사람들은 영어로 검색을 해서 익히면 될 것이다. 잘 설명돼 있는 영어 튜토리얼은 상당히 많기 때문이다. 앞으로는 이걸 직접적으로 어떻게 작성하는지, 실제 예를 바탕으로 해 보자.

### 예제 소스파일

예제 소스파일은 내 github에서 다운받을 수 있다. 아래 커맨드로 최신 버전을 받을 수 있다.

    git clone http://github.com/mytory/wp-publisher.git

clone한 뒤에 학습을 위해 기본 세팅을 해 둔 버전으로 checkout하고 튜토리얼을 시작하면 된다. 아래 명령을 치면 된다.

    git checkout standby

git 사용에 익숙치 않다면 다운로드도 할 수 있다.

* 학습을 시작할 때는 [학습을 위해 기본 세팅을 해 둔 버전](https://github.com/mytory/wp-publisher/tree/standby)을 다운받으면 된다. 
* 완성 버전을 받으려면 [최신 버전](https://github.com/mytory/wp-publisher)을 다운받으면 된다.

위 소스 파일은 워드프레스 3.5.1로 돼 있으며, `wp-config.php`는 포함하고 있지 않다. 기본적인 서버 세팅은 알아서 해야 한다. 위 소스 파일은 기본적인 워드프레스 버전에 `mytory-publisher`라는 테마만 추가한 것이다.

각 소스를 설명할 때는 commit checksum을 표기해 뒀다. 이 경우 git가 연결된 상태에서 아래처럼 쓰면 해당 단계로 소스가 세팅된다. 아래 코드에서는 checksum을 a1s2d3으로 가정했다.

    git checkout a1s2d3

주요 부분에서는 git tag 이름을 써 뒀는데 이 경우에도 해당 tag로 checkout하면 소스 코드가 해당 tag로 세팅된다.

    git checkout tag_name