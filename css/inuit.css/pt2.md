- Date: 2013-09-02 16:14:31

보조 클래스
--------------

	1   .one-whole
	
	1/2 .one-half 
	
	1/3 .one-third
	2/3 .two-thirds
	
	1/4 .one-quarter
	2/4 .two-quarters
	3/4 .three-quarters
	
	1/5 .one-fifth
	2/5 .two-fifths
	3/5 .three-fifths
	4/5 .four-fifths
	
	1/6 .one-sixth
	2/6 .two-sixths
	3/6 .three-sixths
	4/6 .four-sixths
	5/6 .five-sixths
	
	1/8 .one-eighth
	2/8 .two-eighths
	3/8 .three-eighths
	4/8 .four-eighths
	5/8 .five-eighths
	6/8 .six-eighths
	7/8 .seven-eighths
	
	1/10 .one-tenth
	2/10 .two-tenths
	3/10 .three-tenths
	4/10 .four-tenths
	5/10 .five-tenths
	6/10 .six-tenths
	7/10 .seven-tenths
	8/10 .eight-tenths
	9/10 .nine-tenths
	
	1/12 .one-twelfth
	2/12 .two-twelfths
	3/12 .three-twelfths
	4/12 .four-twelfths
	5/12 .five-twelfths
	6/12 .six-twelfths
	7/12 .seven-twelfths
	8/12 .eight-twelfths
	9/12 .nine-twelfths
	10/12 .ten-twelfths
	11/12 .eleven-twelfths
	
	pull--
	push--
	
	.brand
	.brand-face
	.brand-color

- .fr: float right
- .fl: float left
- .text-center
- .full-bleed: .island의 내용을 꽉차게
- .informative: help 커서
- .proceed: "더 보기"용 오른쪽 정렬
- .go:after: 뒤에 "»"를 붙여줌 
- .caps: 대문자화
- .accessibility: `display: none`을 쓰지 않고 숨기기

디버그
------

- 황색 경보
	- 빈 요소
	- `img` 요소에 `alt` 어트리뷰트가 없는 경우
	- `a` 요소에 `title` 어트리뷰트가 없는 경우
	- `a`의 `href`가 `#`이거나 `javascript`를 포함한 경우
	- `a` 요소에 `target` 어트리뷰트가 있는 경우
	- `th` 요소에 `scope` 어트리뷰트가 없는 경우
	- `table`의 새끼 중에(후손 말고) `tr`인 경우
	- `tbody`가 `tfoot`보다 먼저 오는 경우
	- 인라인 스타일이 있는 요소
	- `id`가 있는 요소
- 적색 경보
	- `ol`,`ul` 속에 `li`가 아닌게 있을 경우
	- `form` 요소에 `action` 어트리뷰트가 없는 경우
	- `input` 요소에 `type`이 없는 경우
	- `textarea`에 `row`와 `cols`가 없는 경우
	- `input`의 `type`가 `submit`인데 `value`가 없는 경우
	- `.gw`의 새끼 중에 `.g`가 아닌 녀석이 있는 경우