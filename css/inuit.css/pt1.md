기본 파일 구조

	\
	 + watch.sh
	 + _vars.scss
	 + your-project.scss
	 + inuit.css
		\+ inuit.scss
		 + partials
			\+ base
			 + generic
			 + objects
Generic
--------

### Mixins ###

다음 믹신은 수직적 리듬을 일정하게 유지하기 위한 것이다. 폰트 크기(`$font-size`)를 넘기면, 줄 높이를 언제나 `$base-line-height`의 정수배가 되도록 계산한다.

	@mixin font-size($font-size){
	    font-size:$font-size +px;
	    font-size:$font-size / $base-font-size +rem;
	    line-height:ceil($font-size / $base-line-height) * ($base-line-height / $font-size);
	}

그 다음엔 제조사별 접두어와 CSS 애니메이션을 위한 믹신이 나온다. 다음과 같이 쓰면 된다.

	@include vendor($property, $value);
	@include keyframe($animation-name);

마지막으로 말줄임표를 위한 믹신이다. 너비를 인수로 넘겨주면 된다. 

	@include truncate($truncation-boundary);

### Reset ###

inuit.css는 리셋 CSS를 차용했지만, 너무 고지식한 리셋은 없애버렸다. 그런 예로는: 

- 목록 스타일 리셋
- `big`, `center`, `acronym`처럼 폐기될 요소들의 리셋
- `fieldset` 테두리 리셋
- 폰트 굵기 및 스타일 리셋
- `u`와 `a`에서 밑줄 제거

기본 스타일을 재정의한 것도 있다.

- 양식 요소에서 글꼴을 상속하게 함
- `img`의 글꼴 스타일을 지정해서, 이미지 로드 실패시 나타나는 `alt` 텍스트가 본문과 구별되게끔 함
- 그 밖에 텍스트 관련 요소들

저자는 빈번히 덮어쓰는 리셋은 제거할 것을 촉구한다. 

### Clearfix ###

inuit.css에서는 "micro clearfix"라는 방법을 쓴다. CSS로 요소의 앞과 뒤에 `display`가 `table`인 가짜 요소를 만들어서 위쪽 여백이 접히는 것을 막고, 요소 뒤쪽을 `clear`해주는 방식이다. Firefox 3.5 이상, Safari 4 이상, Chrome, Opera 9 이상, IE 6 이상에서 모두 지원된다. 다만 IE6, 7에서  hasLayout 프로퍼티를 켜고 부동(`float`) 요소를 포함하게 하려면 `*zoom: 1;`을 선언해야 한다. 

inuit.css에서는 `cf` 클래스를 를 일일이 붙여주기 보다는, 그 클래스를 확장하라고 한다.

(익스플로러에서는 요소마다 hasLayout이라는 프로퍼티가 있어서 이것이 `true`이면 조상 요소에 의존하지 않고, 독자적으로 자신과 후손 요소들의 크기와 위치를 정하고, `false`면 조상 요소에 의존한다고 한다. 마이크로소프트는 "성능과 단순함"을 위해 이 프로퍼티를 도입했다고 하지만, 이것은 오히려 상황을 복잡하게 만들었다고 한다. `hasLayout`이 `false`인 `div`에 부동 요소가 있거나 절대 위치로 위치를 잡은 요소가 있으면, 내용이 사라지거나 이상하게 배치되거나, 창을 움직이거나 스크롤할 때 제대로 렌더링이 되지 않는 등 다양한 문제가 발생한다고 한다. 이런 문제는 `hasLayout`을 켜는 CSS 규칙을 선언함으로써 해결할 수 있다고 하는데, 그 중 하나가 `zoom`이다. 참고: <http://reference.sitepoint.com/css/haslayout>)

(여백 접힘이란, 두 요소의 수직 여백이 맞닿아 있을 때에는 더 큰 여백이 살아남고, 작은 여백은 0으로 접힌다는 것이다. 부모와 자식 요소일 경우에도 여백이 맞닿기만 하면 모두 적용된다. 심이나 테두리를 넣으면 여백이 접히는 일을 막을 수 있다.(그런데 익스플로러 7까지는 `hasLayout` 프로퍼티가 있으면 여백이 아예 접히지 않았다고 한다). 다만 작은 여백이 음수일 경우에는 두 여백을 둘 다 적용한다. 부동 요소, 절대 위치 요소, 줄글-덩어리 요소, `overflow`가 `visible`이 아닌 요소, 루트 요소에서는 여백 접힘이 발생하지 않는다. 참고 <http://reference.sitepoint.com/css/collapsingmargins>

### Shared ###

여기에는 몇몇 요소들과 객체의 여백이 지정돼 있다.

여기서 저자는 한 쪽 방향으로만(`margin-bottom`과 `margin-left`로만) 여백을 지정한다. 한 쪽 방향으로만 지정할 때의 이점은

- 수직적 리듬을 규정하기 더 쉽다
- 컴포넌트를 없애거나 이동시켜도 일정한 여백이 보장된다.
- 따라서 컴포넌트들이 나열되는 순서에 종속되지 않게 된다. 
- 여백 접힘을 신경쓰지 않아도 된다. 

여기서 지정된 여백의 값은 모두 $base-spacing-unit이며, $base-font-size를 기준으로 한 `rem` 값을 쓴다. 기본 여백이 지정된 요소와 객체들은 다음과 같다. 

- margin-bottom=$base-spacing-unit
	- `h1`~`6`, `hgroup`, `ul`, `ol`, `dl`, `blockquote`,
	   `p`, `address`, `table`, `fieldset`, `figure`, `pre`
	- `.media`, `.island`, `.islet`
	- `%sass-margin-bottom`
	- `.islet & .islet`(절반)
	- `.landmark`(2배)
	- `hr`(줄의 두께인 2픽셀만큼만 뺌)
- margin-left=$base-spacing-unit: `ul`, `ol`, `dd`

Base
----

### Main ###

html요소의 스타일이다. $base-font-size가 여기에 적용된다.

### Headings ###

저자는 니콜 설리반의 아이디어를 따라 제목에 해당하는 클래스를 모두 만든다. 그렇게 하면 사이드 바에 있는 `h2`가 `h3`처럼 보이게 하고 싶다거나, 푸터에 있는 `h3`가 `h4`처럼 보이게 하고 싶을 때 코드를 줄일 수 있다고 한다. 제목 클래스의 이름은 그리스식 알파벳에서 따왔다.

	h1,.alpha   {}
	h2,.beta    {}
	h3,.gamma   {}
	h4,.delta   {}
	h5,.epsilon {}
	h6,.zeta    {}

각 제목의 크기는 `$h1-size`...`$h2-size`이라는 변수로 설정하며, inuit.css는 이 값을 `font-size`라는 믹신의 인수로 넘긴다. 

`.hN` 클래스는 `hgroup` 요소나 수준이 딱히 정해지지 않은 제목 요소에 사용한다. 

	<hgroup>
		<h1 class="hN>inuit.css</h1>
		<h2 class="hN>Best Framework. Ever!</h2>
	</hgroup>

`.giga`, `.mega`, `.kilo`는 기존 제목 수준(`h1`...`h6`, `.alpha`...`.zeta`)에서 벗어나는 예외적인 커다란 제목에 쓰는 클래스다. 각각의 크기는 `$giga-size`, `$mega-size`, `$kilo-size`라는 변수로 설정하고, `font-size`라는 믹신으로 넘어간다. 

### Paragraph ###

기사 첫 머리에 들어가는 요점이 들어간 한 두 문장짜리 문단을 Lede(Lead와 발음이 같고, 활판 인쇄에 쓰이는 Lead와 구분하기 위해 스펠링을 바꿨다고 한다)라고 한다. `.lede`나 `.lead`는 그런 문단에 쓰이는 클래스이며, 일반 문단보다 글꼴 크기가 12.5퍼센트 크다.

### Small Print ###

여기 있는 규칙들은 작은 글씨에 적용한다. `small` 요소는 `font-size`의 단위가 `em`이어서 언제나 부모 요소보다 글꼴이 작다. `.giga`, `.mega`, `.kilo`처럼 예외적인 글꼴(이번에는 예외적으로 작은 경우다)은 `.smallprint`,`.milli`,(글꼴 크기 변수 `$milli-size`)`.micro`(`$micro-size`)을 쓴다. 

### Quote ###

인용 관련 요소인 `q`와 `blockquote`에 적용되는 규칙들이 여기서 선언된다. `q`로 둘러싸면 홅따옴표로 둘러싸고, 한번 더 `q` 둘러싸면 쌍따옴표로 둘러싼다. inuit.css에서 기본 설정을 덮어쓰지 않고 `q`로 쌍따옴표를 붙이는 방법은 없는듯하다.

`blockquote` 요소를 쓰면 쌍따옴표를 쳐준다. 

   <blockquote>
       <p>Insanity: doing the same thing over and over again and expecting
       different results.</p>
       <b class=source>Albert Einstein</b>
   </blockquote>

`.source` 클래스는 "—"를 본문 앞에 붙여준다. `p`요소의 본문 첫줄은 쌍따옴표 너비만큼 내어쓴다(음수로 지정한 들여쓰기, `text-indent`). `p`요소에 자동으로 붙는 쌍따옴표는 요소 덩어리가 아니라 그 안에 있는 텍스트에 자동으로 붙는다. 이것은 `:last-of-type` 선택자를 활용한 것으로 IE8까지는 지원이 안된다고 한다.

### Code ###

코드를 인용하는 스타일에 대한 것인데, 당장은 쓸 일이 없을 것이므로 생략하겠다. 선언된 클래스: `.code-comment`, `.numbered`, `.numbered__numberes`, `.numbered__numberes code`

### Link ###

inuit.css는 기본적으로 링크에서 밑줄을 없앴고, 마우스를 올리면 밑줄이 생기게 했다. 저자는 마우스를 올릴 때 부정적 반응(뭔가 없어진다거나, 더 흐려진다거나 하는 등의 반응)이 아니라 긍정적 반응(뭔가 생긴다거나, 더 진해진다거나 하는 등)을 보이도록 해야 한다고 주장한다.

다만 이렇게 할 경우 색맹에게는 링크가 구별이 잘 안 될 수도 있다고 말하며, 이럴 때를 대비해서 링크와 본문의 색상 대비를 높이라고 주문한다.

`.current`는 링크 목록에서 현재 페이지를 나타낼 때 쓰는 클래스이며, 밑줄이 들어가 있고, 마우스를 올려도 커서는 `text`로 유지된다.

### Image ###

`img` 요소는 너비가 `100%`를 넘지 않게끔 돼 있다. 

	img{
	    max-width:100%;
	    height:auto;
	}
	figure > img{
	    display:block; /* 원래는 inline이다 */
	}

### List ###

중첩된 목록은 수직 여백(inuit.css에서는 `margin-bottom`)을 없앴다. 

### Table ###

기본 표 스타일은 다음과 같다. 

	table{ width:100%; }
	th,
	td{
	    padding:$base-spacing-unit / 4 +px;
	    @media screen and (min-width:480px){
	        padding:$half-spacing-unit +px;
	    }
	    text-align:left;
	}
	
	[colspan]{ text-align:center; }
	[colspan="1"]{ text-align:left; }
	[rowspan]{ vertical-align:middle; }
	[rowspan="1"]{ vertical-align:top; }
 	.numerical{ text-align:right; }
	
	.t5     { width: 5% }
	.t10    { width:10% }
	.t12    { width:12.5% }     /* 1/8 */
	.t15    { width:15% }
	.t20    { width:20% }
	.t25    { width:25% }       /* 1/4 */
	.t30    { width:30% }
	.t33    { width:33.333% }   /* 1/3 */
	.t35    { width:35% }
	.t37    { width:37.5% }     /* 3/8 */
	.t40    { width:40% }
	.t45    { width:45% }
	.t50    { width:50% }       /* 1/2 */
	.t55    { width:55% }
	.t60    { width:60% }
	.t62    { width:62.5% }     /* 5/8 */
	.t65    { width:65% }
	.t66    { width:66.666% }   /* 2/3 */
	.t70    { width:70% }
	.t75    { width:75% }       /* 3/4*/
	.t80    { width:80% }
	.t85    { width:85% }
	.t87    { width:87.5% }     /* 7/8 */
	.t90    { width:90% }
	.t95    { width:95% }

여기에서 확장되는 테이블용 클래스는 다음과 같다.

- `.table--bordered`: 테두리 있는 표
- `.table--striped`: 아무 스타일이 없는 표
- `.table--data`: 데이터용 표(글꼴 크기가 2/3다).

### Forms ###

1. `input[type=text]`나 `input[type=email]`, `input[type=password]` 등은 `type` 어트리뷰트를 불문하고 모두 텍스트 입력란으로 나온다. 그래서 텍스트 입력란은 `[type]` 선택자가 아니라, `.text-input`이라는 클래스로 스타일을 매긴다. 
2. 여러 필드는 목록으로 처리한다. 하나의 항목(`li` 요소)에는 `label` 요소(`display`는 `block`임)와 `input` 요소가 차례대로 들어간다. 
3. `label`과 동일한 효과를 내는 `.label`이라는 클래스가 있어서, `label` 요소를 쓰기에는 부적절하지만 라벨의 기능을 하는 요소에 적용할 수 있다. (가령 4지선다 문항에서, 질문에 해당하는 부분)
4. 체크리스트 목록은 `.check-list` 안에 넣는다. 
5. "내 이름은 __입니다"와 같은 구어체 입력란의 경우에는 라벨을 `.spoken-form`의 자식 요소로 둔다. 그럴 경우 라벨은 `block`이 아니라 `inline-block`이 된다. 
6. `.additional`: 라벨에 들어가는 추가적인 도움말에 적용한다.
7. `.extra-help`: 앞에 나오는 `input` 요소가 포커스 되거나 활성화되면 나타나는 도움말 메시지

Objects
-------

### grid ###

격자는 기본적으로 `.grid-wrapper`(줄여서 `.gw`)로 둘러싼다. 기본적으로는 왼쪽에서 오른쪽으로 격자를 채우지만, `.grid-wrapper--rev`(줄여서 `.gw--rev`)를 덧붙이면 오른쪽에서 왼쪽으로 격자를 채운다. 

### flexbox ###

Flexbox는 향후 CSS에 포함될 레이아웃 모드이며, `display`가 `flexbox`인 다양하게 배치할 수 있다(가로, 세로, 가운데 정렬(세로로도), 공간의 균등 분할, 심지어 순서도 바꿀 수 있다고도 한다). 

다만 inuit.css에서는 그냥 `display`가 `table`인 요소이며, 이 모듈 안에 들어간 요소들은 공간을 균등하게 분배 받는다.

### text-cols ###

컬럼을 구현한 모듈인데, 익스플로러는 전혀 지원하지 못하는 듯하다. 

### nav ###
