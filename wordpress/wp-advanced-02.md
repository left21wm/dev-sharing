## custom post type - book을 만들어 보자

### 기본 개념

워드프레스에서 내용을 가진 모든 것은 `post`로 불린다. 그리고 이 `post`엔 post type이 있어서 분류가 된다. 워드프레스에서 기본적으로 사용하는 `post_type`엔 다음과 같은 것들이 있다.

* attachment : 첨부 파일
* nav_menu_item : 메뉴 아이템
* page : 페이지
* post : 글
* revision : post와 page의 과거 버전 기록

위 값들은 `wp_posts` 테이블의 `post_type` 컬럼에 들어간다. (물론 테이블의 prefix는 설정에 따라 달라질 수 있다.)

이 모든 것들인 일단 post고, 이것들의 `post_type`에 따라 용도가 정해지는 것이다. post의 `post_type`이 `post`고, page의 `post_type`이 `page`라고 말하는 게 좀 이상하게 들릴 수 있겠지만 말이다. 그럼 우리는 book이라는 `post_type`을 만들어서 책을 입력하면 되겠다.

### 등록하기

기본적인 내용은 [Codex의 register_post_type 함수 레퍼런스](http://codex.wordpress.org/Function_Reference/register_post_type)를 참고하면 된다.

다운받은 예제 파일을 보면 `functions-custom-post-type.php`라고 있을 것이다. 이 파일은 `functions.php`에 `include`돼 있다.

여기에 아래 코드를 넣어 보자. 코드는 차분히 설명할 테니 일단 따라해 보자.

    //===== book custom post type =====

    function mpub_custom_post_type() {
      $args = array(
        'label' => "책",
        'public' => true,
        'has_archive' => true,
        'menu_position' => 5,
      );

      register_post_type( 'book', $args );
    }
    add_action( 'init', 'mpub_custom_post_type' );

이렇게 하고 관리자 페이지에 들어가 보면, '책'이라는 메뉴가 생긴 것을 알 수 있다.

![관리자 메뉴에 책 항목이 추가된 모습](img/img01-admin-book-menu-simple.png)

### `add_action`이란?

일단 직접적으로 등록을 하는 부분은 바로 아래 코드다.

    add_action( 'init', 'mpub_custom_post_type' );

워드프레스를 초기화하는 단계(`init`)에 `mpub_custom_post_type`을 실행하라고 등록해 주는 것이다. 워드프레스는 엄청나게 많은 사이트에서 사용하는 플랫폼이다. 워드프레스의 기본적인 실행 코드(코어 파일)를 변경하지 않으면서도 중간중간에 개발자가 자신의 함수를 끼워넣을 수 있도록 해야 한다. 그걸 하도록 해 주는 게 바로 `add_action`과 `add_filter` 함수다. 

워드프레스 코어 파일을 살펴 보면 곳곳에 `do_action( 'keywod' )`와 `apply_filter( 'keyword' )`라는 코드를 볼 수 있다. 개발자가 `add_action`과 `add_filter`로 등록한 함수를 바로 그 지점에서 실행하는 것이다.

루트 폴더의 `wp-setting.php` 파일의 299~306번째 줄을 살펴 보면 아래와 같은 코드가 있다. 우리가 등록한 `mpub_custom_post_type` 함수가 바로 여기에서 실행되는 것이다.

    /**
     * Most of WP is loaded at this stage, and the user is authenticated. WP continues
     * to load on the init hook that follows (e.g. widgets), and many plugins instantiate
     * themselves on it for all sorts of reasons (e.g. they need a user, a taxonomy, etc.).
     *
     * If you wish to plug an action once WP is loaded, use the wp_loaded hook below.
     */
    do_action( 'init' );

