PhpUnit 설치와 사용법
----

패키지 관리자를 쓸 수도 있지만, 일단은 가장 간단한 방법: 

	$ wget http://pear.phpunit.de/get/phpunit.phar

이 파일을 테스트 케이스 파일을 인자로 넘겨서 실행하면 테스트를 수행한다. 일단 실행하기 편하도록 실행 권한을 주고 `$PATH`에 등록된 경로에 집어넣는다.

	$ chmod +x phpunit.phar
	$ mv phpunit.phar ~bin/phpunit

그러면 다음과 같은 식으로 사용할 수 있다.

	$ phpunit [테스트 케이스]

테스트 케이스
----

phpunit에서 테스트 케이스는 php 클래스이고, `PHPUnit_Framework_TestCase`의 하위 클래스다.

	class MyTest extends PHPUnit_Framework_TestCase {
	}

각 테스트는 `test`로 시작하는 메서드다. 메서드 이름은 무엇을 테스트하는지 알 수 있도록 쓴다. 완전한 문장으로 쓰는 사례도 많이 볼 수 있다.

	class MyTest extends PHPUnit_Framework_TestCase {
		public function test_equality () {
			$this->assertTrue(1 == 1);
		}
	}

각 테스트에는 검증돼야할 진술(Assertion)이 있다. 이 진술이 모두 참으로 판명 나면 테스트는 성공한다. phpunit에는 `assert`로 시작하는 각종 메서드가 있어서 이 메서드로 참/거짓 여부를 가른다. 위의 경우에는 `assertTrue`가 호출됐고, 이 메서드는 인자의 값이 `True`여야 테스트가 성공할 수 있다. `assertFalse`나 `assertEquals` 등도 이와 비슷하다.

`MyTest.php`로 저장하고 테스트 케이스를 한 번 테스트해보자.

	phpunit MyTest.php

결과물은 대략 다음과 같다.

	PHPUnit 3.7.24 by Sebastian Bergmann.

	.
	
	Time: 2 ms, Memory: 5.25Mb

	OK (1 test, 1 assertion)

세번째 줄에 나오는 `.`는 통과된 테스트에 대응된다. 테스트가 여럿 있고 모두 통과하면 `.....`과 같이 나올 것이다. 통과하지 못한 테스트는 `F`로 나온다. 

한 테스트에 진술이 여러개 있을 수도 있다. 그런 경우 모든 진술이 참으로 판명돼야 한다.

### 테스트의 정석 ###

- 그러나 일반적으로는 테스트 하나에 진술을 하나씩 검증하는 것이 좋다. 그래야 테스트가 실패하면 무엇이 문제인지 바로 알 수 있기 때문이다.
- 테스트는 진술이 반드시 검증되게끔 작성해야 한다. 그러므로 `if` 구문은 피해야 한다.

### `assert-` 메서드들 ###

- `assertTrue`, `assertFalse`
- `assertEquals($expected, $actual)`, `assertSame($expected, $actual)`(둘의 차이는 `==`와 `===`의 차이라고 생각하면 된다)
- `assertContains($needle, $haystack)`, `assertNotContains($needle, $haystack)`
- `assertArrayHasKey($key, $array)`
- `assertContainsOnly($type, $variable)`
- `assertType($type, $var)`: 타입 검사
- `assertNotNull`
- `assertLessThan`, `assertGreaterThan`
- `assertStringsStartWith($prefix, $str)`, `assertStringsEndsWith($suffix, $str)`

붙박이(fixture)
-----

테스트를 작성하다보면 테스트가 공유하는 환경이 있을 수 있다. 가령 클래스를 테스트한다면 언제나 인스턴스를 만들 것이다. 가짜 데이터 베이스를 만드는 테스트 케이스라면 각 테스트를 마치고 데이터 베이스를 날려야 할 것이다. 모름지기 프로그래머란 매 테스트 케이스마다 같은 코드를 매번 작성하는 부지런을 떨어선 안되는 존재다.

여러 테스트가 공유하는 코드를 '붙박이'(fixture)라고 한다. phpunit에서 붙박이는 `setUp()`과 `tearDown()` 메서드로 정의된다. 두 메서드는 각 테스트가 수행되기 전과 후에 호출된다. 기본 테스트 케이스 클래스에서는 그냥 빈 메서드고, 필요할 경우 테스트 케이스용 클래스에서 덮어 쓰면 된다.

	class FixtureTest extends PHPUnit_Framework_TestCase {
		private $_instance;
		public function setUp () {
			$this->_instance = new SomeClass('arg1', 'arg2');
		}
		public test1 {
			// ... $this->_instance로 테스트를 한다. ...
		}
		public test2 {
			// 마찬가지 ...
		}
	}

- 보통은 다음번 `setUp`을 호출하면서 기존 객체가 수거되기 때문에 `tearDown()`은 빈 메서드로도 충분한 경우가 많다.
- `setUp`은 매 테스트 마다 호출된다. 다른 붙박이가 필요하면 테스트 케이스를 따로 작성하는 게 보통이다. 다른 시나리오 하에서의 테스트인 만큼 테스트 케이스도 따로 작성하는 것이다. 
- 여러 테스트 케이스가 붙박이를 공유하면 코드가 구리다는 징후일 수 있다. 클래스가 잘 분리되지 않으면 그런 일이 벌어진다. 
- `setUpBeforeClass()`와 `tearDownAfterClass()`는 테스트 케이스 시작 전과 후에 호출되는 메서드다.

주석(annotations)
----

phpdoc 형식의 주석으로 반복되는 코드를 더 줄일 수도 있다.

예를 들어 예외를 제대로 토해내는지 검사하는 테스트가 있다.

	public function test_exception() {
		try {
			$iterator = new ArrayIterator(42);
			$this->fail();
		} catch (InvalidArgumentException $e) {
		}
	}

표준 PHP 라이브러리(SPL)에 포함된 `ArrayIterator` 클래스는 생성자의 인자로 정수를 받을 수 없다. 그래서 `new ArrayIterator(42)`는 예외를 토해내고 그 다음 구문이 실행되지 않는다. 예외를 토해내지 않으면 `$this->fail()`(이것은 `$this->assertTrue(false)`와 같다)이 실행되고 테스트는 실패로 끝난다. 

예측을 불허하는 사용자의 입력값을 다루는 클래스의 경우 예외를 제대로 토해야 탈이 없다. 그래서 위와 같은 테스트가 종종 필요하다. 그런데 위 코드는 그 의도가 명확히 드러나지는 않는다. 진술을 검증하는 코드가 실행되지 않아야 통과하는 테스트이기 때문이다. 게다가 비어있는 `catch` 구문을 써야 한다는 것도 성가시다.

그러나 phpdoc 형식으로 주석을 달아서 이런 코드를 줄일 수 있다.

	/**
	 * @expectedException InvalidArgumentException
 	 */
	public function test_exception() {
		$iterator = new ArrayIterator(42);
	}

`@expectedException`으로 어떤 예외를 토해낼지를 쓰면 알아서 phpunit은 알아서 그에 맞게 알아서 테스트를 수행한다. 

다양한 입력 값을 시험해보고 싶다면 `@dataProvider`를 쓰면된다. 다양한 값의 참여부를 알아내는 간단한 테스트가 있다.

	public function test_truthness () {
		$values = array(1, '1', 'hello', true);
		foreach ($values as $value) {
			$bool = (bool) $value;
			$this->assertTrue($actual);
		}
	}

`foreach` 구문은 번거롭다. `@dataProvider` 태그가 있는 주석을 달면 의도가 더 분명한 코드를 쓸 수 있다. 우선 `$values`에 해당하는 배열을 돌려주는 정적 메서드를 정의하고, 그 메서드를 테스트 메서드의 주석에서 `@dataProvider` 태그를 달아 언급한다.

	public static function values() {
		return array(
			array(1), 
			array('1'), 
			array('on'),
			array(true)
		);
	}

	/**
	 * @dataProvider values
	 */
	public function test_truthness ($value) {
		$bool = (bool) $value;
		$this->assertTrue($actual);
	}

이렇게 하면 `@dataProvider`로 지정한 정적 메서드가 돌려준 배열의 각 요소로 테스트를 실행한다. 각 요소 또한 배열인데, 이것은 테스트 메서드의 인자로 넘어간다. `func_get_args()`를 출력해보면 확인해 볼 수 있을 것이다.

주석으로 불가피한 테스트 의존성을 해결할 수도 있다. 가장 좋기로는 모든 테스트가 순서에 상관 없이 독립적으로 실행될 수 있어야 하지만 그럴 수 없는 때가 있다. 가령 어떤 클래스에서 `add()` 메서드와 `delete()` 메서드를 테스트하는데, `delete()` 메서드는 일단 `add()`가 제대로 작동해야 테스트할 수 있는 상황이 있을 수도 있다. 이럴 경우 `@depends` 태그를 달아서 의존하는 테스트를 적어주면 된다.

	public function test_set() {
		$arr = array();
		$arr[0] = 'foo';
		$this->assertTrue(isset($arr[0]));
		return $arr;
	}
	/**
	 * @depends test_set
	 */
	public function test_unset($arr) {
		unset($arr[0]);
		$this->assertFalse(isset($arr[0]));
	}

`test_unset`는 `test_set`에 의존하는 테스트다. `@depends` 태그를 달면, 거기에 언급된 테스트가 돌려주는 값을 인자로 받아서 테스트를 수행한다. 

To be continued...
--------

다음 시간에는 Stub과 Mock 개념을 다루겠다.
