#  @RequestBody vs @RequestParam

컨트롤러에서 데이터를 인자에 할당하는 대표적인 방법으로는 @RequestBody 와 @RequestParam 이 있다.

```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestParam String name) {
		System.out.println("통신 성공");
		System.out.println(">>> " + name);
		return "index";
	}
}

@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestBody String name) {
		System.out.println("통신 성공");
		System.out.println(">>> " + name);
		return "index";
	}
}
```

## form태그로 데이터를 전달
![Form Tag](/mobile_programming/images/form%20tap.png)

데이터를 전송하여 컨트롤러에서 @RequestBody를 이용하여 데이터를 받아보았다.

### Request Body로 받기
```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestBody String name) {
		System.out.println("통신 성공");
		System.out.println(">>> " + name);
		return "index";
	}
}
//>>> name=jun&age=13
```

`'name=jun&age=13'`  
> 'jun' 이라는 이름과 '13' 이라는 나이가 잘 전달이 되었지만 단지 'name=jun&age=13' 이라는 String 으로 전달되어 전달된 데이터를 사용하기에는 불편함이 있다.

### RequestParam으로 받기
```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestParam String name) {
		System.out.println("통신 성공");
		System.out.println(">>> " + name);
		return "result";
	}
}

//>>> jun
```

> @RequestBody 로 데이터를 받을 때는 메서드의 변수명이 상관이 없었지만, @RequestParam 으로 데이터를 받을때는 데이터를 저장하는 이름으로 메서드의 변수명을 설정해주어야 한다.  결과적으로 jun 이라는 이름이 잘 전달이 되었고, 이번엔 name 이라는 변수에 할당이 되어 사용하기에도 용이하다.

### Json형식으로 데이터를 전달
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button id="postData">
    데이터 전송
</button>
<script>
    const $postDataButton = document.querySelector("#postData")

    const postData = event => {
        console.log("데이터 전송");

        fetch("/receive", {
            method: 'post',
            headers: {
                'content-type': 'application/json'
            },
            body : JSON.stringify({
                name : "jun",
                age : "13"
            })
        })
    }

    $postDataButton.addEventListener("click", postData);
</script>
</body>
</html>
```

`홈페이지`
![Alt text](/mobile_programming/images/page.png)
> '데이터 전송' 버튼을 누르면 '/receive' 주소로, post 방식으로, *{name : "jun", age : "13}* 이라는 데이터가 Json의 형태로 전송이 된다.

이 데이터를 @RequestParam 을 이용하여 받아보자.

```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestParam String name) {
		System.out.println("통신 성공");
		System.out.println(">>> " + name);
		return "result";
	}
}
```
 
그치만 이렇게 실행하면 오류가 발생한다.
* MissingServletRequestParameterException: Required String parameter 'name' is not present

name 이라는 파라미터가 없다고 한다.

그 이유는 기본적으로 @RequestParam 은 *url 상에서 데이터를 찾기 때문*이다.
우리가 위에서 <form> 태그를 이용하여 데이터를 입력하고 제출 버튼을 누르면 입력한 데이터들이 url을 통해서 전달된다.

예를 들면 *'http://localhost:8080/receive?name=jun&age=13'* 이런 식이다.

반면에 Json형식으로 데이터를 전달할때는, url은 http://localhost:8080/receive로 변함이 없고 *body에 데이터를 포함*하여 전송하기 때문에 @RequestParam 으로는 받을 수 없는 것이다.


#### '/receive' 주소로 데이터를 전송하고 @RequestBody 로 데이터를 받아보자.
```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestBody String req) {
		System.out.println("통신 성공");
		System.out.println(">>> " + req);
		return "result";
	}
}
```

## 자동 객체 생성
만약 다음과 같이 name 과 age 를 필요로 하는 Person 클래스가 있고 getter 가 구현되어 있다면
```java
public class Person {
    private String name;
    private int age;

    public Person() {

    }

    public Person(final String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

다음과 같은 기능이 가능하다.
```java
@Controller
public class UserController {

	@PostMapping("/receive")
	public String age(@RequestBody Person person) {
		System.out.println("통신 성공");
		System.out.println(">>> " + person);
		return "result";
	}
}
//>>> Person{name='jun', age=13}
```
> 신기하게도 Person 객체를 자동으로 생성해 주었다.
> @RequestBody 가 아닌 @RequestParam 을 이용한다면 불가능하다. 한번 시도해보자.

## 정리
![결론](/mobile_programming/images/summary.png)

url상에서 데이터를 전달하는 경우(form 태그 등) @RequestParam 을 이용하고,
그 외의 경우 @RequestBody 를 이용하자!