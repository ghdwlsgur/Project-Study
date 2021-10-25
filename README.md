### 😎 10/21 (목)
<hr>
## 쿠키와 세션을 사용하는 이유
> HTTP 프로토콜의 특징이자 약점을 보완하기 위해서 사용.


## HTTP 프로토콜 특징
- 비연결지향(Connectionless)
1. HTTP는 클라이언트가 요청(request)을 서버에 보내고, 서버는 클라언트에게 적절한 응답(response)을 주고<br>
연결을 끊는 특성이 있다.
2. HTTP1.1 버전에서는 커넥션을 계속 유지하고 요청(request)에 재활용하는 기능이 추가되었다.(HTTP Header)에 <br>
에 keep-alive 옵션을 주어 커넥션을 재활용하게 한다. HTTP1.1 버전에서는 디폴트(default)옵션이다.
- 상태없음(Stateless)
1. 커넥션을 끊는 순간 클라이언트와 서버의 통신이 끝나며 상태 정보는 유지하지 않는 특성이 있다.

HTTP는 이 두가지 특성을 보완하기 위해서 쿠키와 세션을 사용하게 되었다.<br>
비연결지향이라는 특성 덕분에 계속해서 커넥션을 유지하지 않기 때문에 서버 리소스 낭비가 줄어드는 것은 아주 큰 장점이지만, <br>
통신할 때마다 새로 커넥션을 만들기 때문에 클라이언트 측면에서는 상태를 유지를 위해 통신할 때마다 어떤 절차를 가져야 하는 단점이<br>
생긴다.


## 쿠키(Cookie)
쿠키는 클라이언트 로컬(local)에 저장되는 키와 값(key, value)이 들어있는 작은 데이터 파일이다. <br>
쿠키는 서버에서 HTTP Response Header에 Set-Cookie 속성을 이용하여 클라이언트에 쿠키를 제공한다. <br>
쿠키에는 이름, 값, 만료 날짜/시간(쿠키 저장기간), 경로 정보등이 들어있다.<br>
쿠키는 클라이언트의 상태 정보를 로컬에 저장했다가 요청(Request)할 때 참조된다. <br>
쿠키는 서버측에서 만료 날짜/시간을 지정하여 정해진 시간동안 데이터(상태정보)를 유지할 수 있다. (로그인 상태 유지에 활용된다.)<br>

## 세션쿠키(Session Cookie)와 지속 쿠키(Persistent Cookie)

쿠키는 세션 쿠키(Session Cookie)와 지속 쿠키(Persistent Cookie)로 나뉜다.<br>
만료 날짜/시간을 지정하지 않으면, '메모리에 있는 동안' 계속 유효하다고 판단하도록 세션 쿠키에 저장되고, 만료 날짜/시간을 지정<br>
하면 프로세스가 종료되더라도(메모리에서 사라지더라도) 특정 만료날짜/시간까지 유효하므로 지속 쿠키에 저장된다.<br>
세션 쿠키는 브라우저 메모리에 저장되므로 브라우저가 종료되어도 쿠키는 남아있게 된다.<br>
지속 쿠키는 파일로 저장되므로 브라우저가 종료되어도 쿠키는 남아있게 된다.<br>

참고로 세션 쿠키의 값은 보안상 꽤나 안전한 브라우저(크롬 etc)의 메모리에 저장되기 때문에 보안에 유리하지만 파일로 저장되는 <br>
지속 쿠키의 경우 비교적으로 보안에 취약하다. <br>

### 쿠키 프로세스
1. 브라우저에서 웹페이지에 접속한다.<br>
2. 클라이언트가 요청한 웹페이지를 응답으로 받으면서 HTTP 헤더를 통해 해당 서버에서 제공하는 쿠키 값을 응답으로 준다. <br>
3. 클라이언트가 웹페이지를 요청한 서버에 재 요청시 받았던 쿠키 정보도 같이 HTTP 헤더에 담아서 요청한다.<br>
4. 서버는 클라이언트의 요청(Request)에서 쿠키 값을 참고하여 비즈니스 로직을 수행한다.<br>

### 쿠키 사용사례
> 자동 로그인, 팝업 "오늘 더 이상 이 창을 보지 않음", 장바구니 ...

### 쿠키의 한계
- 클라이언트에 최대 300개까지 쿠키를 저장할 수 있다.
- 서버 도메인 하나당 최대 20개의 쿠키를 저장할 수 있다.
- 하나의 쿠키 값은 최대 4KB까지 저장할 수 있다.

> 쿠키는 사용자가 별도로 요청하지 않아도 브라우저(Client)에서 서버에 요청(Request)시에 Request Header에 쿠키 값을 <br>
넣어 요청한다. 그렇다고 그 많은 쿠키 값을 굳이 모든 요청에 넣어 비효율적으로 동작시키지 않는다. 도메인 설정을 통해 <br>
지정한 도메인으로 요청할 때만 쿠키값이 제공되도록 할 수 있다.

## 세션(Session)
서버(Server)에 클라이언트의 상태 정보를 저장하는 기술로 논리적인 연결을 세션이라고 한다. <br>
웹 서버에 클라이언트에 대한 정보를 저장하고 클라이언트에게는 클라이언트를 구분할 수 있는 ID를 부여하는데 이것을 세션아이디라고 한다.<br>

### 세션 프로세스
1. 클라이언트가 서버에 요청했을 때, 필요에 따라 세션에 클라이언트에 대한 데이터를 저장하고 세션아이디 응답을 통해 발급해준다. <br>
2. 클라이언트는 발급받은 세션아이디를 쿠키로 저장한다.<br>
3. 클라이언트는 다시 서버에 요청할 때, 세션 아이디를 서버에 전달하여 상태 정보를 서버가 활용할 수 있도록 해준다. <br>
결과적으로 세션을 통해 클라이언트의 정보는 서버에 두고, 세션아이디를 이용해서 인증받고 정보를 이용하는 방식이다.

## 세션 사용 사례
> 로그인 정보 유지


# 쿠키와 세션의 차이

## 저장 위치
쿠키는 클라이언트(브라우저) 메모리 또는 파일에 저장하고, 세션은 서버 메모리에 저장된다.

## 보안
- 쿠키는 클라이언트 로컬(local)에 저장되기도 하고 특히 파일로 저장되는 경우 탈취, 변조될 위험이 있고,<br>
Request/Response에서 스니핑을 당할 위험이 있어 보안이 비교적 취약하다. 반대로 세션은 클라이언트 정보 자체는 서버에 <br>
저장되어 있으므로 비교적 안전하다.

## 라이프 사이클
- 쿠키는 앞서 설명한 지속 쿠키의 경우에 브라우저를 종료하더라도 저장되어 있을 수 있는 반면에 세션은 서버에서 만료시간/날짜를 정해서<br>
지워버릴 수 있기도 하고 세션 쿠키에 세션 아이디를 정한 경우, 브라우저 종료시 세션아이디가 날아갈 수 있다.

## 속도
- 쿠키에 정보가 있기 때문에 서버에 요청시 헤더를 바로 참조하면 되므로 속도에서 유리하지만. 세션은 제공받은 세션아이디를 이용해서 <br>
서버에 다시 데이터를 참조해야 하므로 속도가 비교적 느릴 수 있다.

진행상황 : 회원가입, 로그인, 게시글 작성 REST API 작성, 테스트 완료 

### 😎 10/22 (금)
<hr>

보다 편리하게 유효성 검사를 진행하기 위해서 리액트 훅을 알아보던 중 useForm을 발견했다. !! <br>
## React Hook Form https://react-hook-form.com/ <br>

> React Hook Form은 React에서 Form을 쉽게 만들기 위한 라이브러리로 성능이 좋고 유연하며 유효성 검사에 탁월하다.

## 장점 <br>
- 적은 코드로 더 좋은 퍼포먼스를 낼 수 있다.
- 다른 라이브러리 혹은 React에 비해 Re-render수가 적다.
- Fast Mounting (로딩속도가 빠름)
- TS를 기본으로 지원

## Register 
> register은 input에서 값을 불러오기 위한 함수로 다른 옵션을 이용하면 input의 유효성 검사도 쉽게 할 수 있다.

먼저 register은 사용하기 위해서는 input에 다음과 같이 {...register("사용하고 싶은 이름")} 이라고 적어주면 <br>
나중에 적은 이름으로 값을 불러올 수 있다. '어떻게 값을 불러올까?' input에서 입력하는 값을 실시간으로 확인하기 위해서는 <br>
watch라는 함수를 사용할 수 있습니다.

## handleSubmit
> handleSubmit은 React Hook Form에서 Submit을 관리하기 위해 만든 함수이다.

handleSubmit은 함수를 인자로 받으며 그 함수에 data라는 인자를 넘겨준다. 이렇게 넘겨받은 데이터를 출력하면 watch 함수가 <br>
가장 마지막으로 출력하는 데이터를 볼 수 있다. 

## onError
> handleSubmit은 두가지 인자를 받는데 하나는 onSubmit으로 정상적으로 Submit 되었을 때 실행하는 함수이고 두번째 인자는<br>
onError로 Form에서 에러가 났을때 실행되는 함수입니다.
>> 여기서 에러는 Validation을 통과하지 못했다는 것을 의미합니다.

## mode: "onChange"
> 실시간으로 유효성 검사를 하게 하며 input에 validation을 설정한 다음에 useForm에서 errors라는 객체를 가져옵니다.
>> errors는 에러들이 담긴 객체로 모드가 onChange일 경우 에러가 실시간으로 업데이트 된다.

## + 폼 양식을 작성할 때는 공식문서에 있는 빌더를 이용하면 편리하다. 👍🏻👍🏻👍🏻 
<img src="https://user-images.githubusercontent.com/77400522/138428898-1f227695-2d62-4fb7-a0de-8cd0da2345df.png" width="100%" height="100%" />

진행상황 : 프론트엔드 로그인, 회원가입, 인증 구현 완료, 유효성 검사(진행)

### 😎 10/23 (토)
<hr>

클라이언트에서 보내는 데이터의 변수명과 디비에서 작성한 데이터의 변수명이 동일한지 확인하고 해당 로직에 관여하는 모든 코드를 살펴보았다... <br>
이름이 다른 변수들을 수정하고 클라이언트에서 요청하는 데이터가 서버에서 주는 데이터보다 더 많은 데이터를 요구하고 있었고 클라이언트에서 <br>
필요한 데이터이므로 데이터를 추가하고 유효성 검사를 기존에는 서버에서 joi 라이브러리를 활용할 계획이었으나 useForm이라는 <br>
훅을 알게되어 이것이 더 사용하기 편리하겠다고 생각하고 사용하게 되었는데 서버에서 작성해둔 joi 유효성 검사조건과 클라이언 <br>
트에서 작성한 유효성 조건이 일치하지 않았고 이 또한 에러의 원인이 되었다.<br>
다른 에러보다 Bad request 400에러가 처리하기 힘들다...가장 만나기 싫은 에러가 되었다<br>

회원가입 -> DB데이터 저장 확인 -> 토큰 미부여 -> 로그인 -> DB 토큰 생성 확인 -> 로그아웃 -> 토큰값 '' 확인


진행상황 : 로그인, 회원가입 bad request 400 error 해결 (프론트엔트 유효성 검사, 백엔드 joi 유효성 검사 충돌), 드롭다운 구성(게시판, 채팅, 문의하기), 헤더 css...


### 😎 10/24 (일)

에러 : 로그인 로직 수행중 비밀번호가 틀렸을 경우 "비밀번호가 틀렸습니다" 에러가 표시되어야 하는 상황에서 에러가 표시되지 않는 문제 <br>
특이점 : 데이터베이스에 가입된 이메일로 틀린 비밀번호를 입력하여 로그인을 실행할 경우 "비밀번호가 틀렸습니다" 정상 실행 <br>
하지만 데이터베이스에 없는 이메일로 로그인을 시도할 경우 bad request 400 발생. <br>
해결 : 로그인 라우터 수정 (이메일이 존재할 경우 status(400)을 전송하였는데 이것이 원인) <br>

## 수정 전 
```javascript
router.post('/login', (req, res) => {
  User.findOne({ email: req.body.email }, (err, user) => {
    // 이메일 존재여부확인 
    if(!user) {
      return res.status(400).json({
        success: false, 
        message: "해당 이메일에 가입된 사용자가 없습니다."
      });
    }
    // 비밀번호 비교 (몽고스키마 인스턴스 메소드)
    user.comparePassword(req.body.password, (err, isMatch) => {
      if(!isMatch)
      return res.json({ success: false, message: "비밀번호가 틀렸습니다." });
    // 토큰 생성 (몽고스키마 인스턴스 메소드)
      user.generateToken((err, user) => {
          if(err) return res.status(400).send(err);
          res.cookie("auth_token", user.token).status(200)
          .json({
            _id: user._id,
            success: true
          });
      })
    })
  });
});

```

## 수정 후 
```javascript
router.post('/login', (req, res) => {
  User.findOne({ email: req.body.email }, (err, user) => {
    // 이메일 존재여부확인 
    if(!user) {
      return res.json({
        success: false, 
        message: "해당 이메일에 가입된 사용자가 없습니다."
      });
    }
    // 비밀번호 비교 (몽고스키마 인스턴스 메소드)
    user.comparePassword(req.body.password, (err, isMatch) => {
      if(!isMatch)
      return res.json({ success: false, message: "비밀번호가 틀렸습니다." });
    // 토큰 생성 (몽고스키마 인스턴스 메소드)
      user.generateToken((err, user) => {
          if(err) return res.status(400).send(err);
          res.cookie("auth_token", user.token).status(200)
          .json({
            _id: user._id,
            success: true
          });
      })
    })
  });
});

```


### 😎 10/25 (월)

에러: 로그인을 하고 나면 토큰에 내가 저장한 사용자 정보 중 사용자 이름을 가져와서 상단바에 입력하고자 하여 useEffect를 사용하여<br>
마운트 시에 한 번만 렌더링시켰으나 로그인 후 새로고침이 되지 않아 이름 정보가 화면에 출력되지 않는 상황, 새로고침을 해야 상단바에 이름이 출력<br>

해결: 로그인 직후에 props.history.push('/')를 이용하여 페이지를 이동시켰으나 새로고침은 되지 않아 window.location.replace('/')<br>
를 사용하여 페이지 이동 후 새로고침 진행하자 화면에 이름이 출력되었다.<br>

키워드: 페이지간 데이터 이동(쿠키), useEffect(), 리액트 생명주기

## history.push vs window.location.href 비교

공통점
- 다른 페이지로 이동

차이점
- HTTP 요청<br>
history.push X | window.location.href O<br>
- 새로고침<br>
history.push X | window.location.href O<br>
- Application 상태 유지<br>
history.push O | window.location.href X<br>


> 좋은 UX와 상태 지속성을 위한다면 일반적인 페이지 이동은 history.push가 더 나은 선택일 수도... <br>
> 이동과 함께 새로고침이 필요한 경우는 window.location.href를 사용.

## push와 replace의 차이점 ??
Home > Item > Login > Item 순으로 페이지를 이동했을 때 Login 페이지에서 history.push / history.replace 사용시 차이점 <br>

- history.push
> Home > Item > Login > Item 순으로 history에 쌓여서 마지막 페이지에서 뒤로가기 버튼을 누르면 Login 페이지로 되돌아간다.

- history.replace
> Home > Item > Item 순으로 history에 쌓여서 마지막 페이지에서 뒤로가기 버튼을 누르면 Item 페이지로 되돌아간다.

history를 스택이라고 가정한다면 push는 history 최상단에 쌓는 것, replace는 history 제일 위에 있는 원소를 지금 넣을 원소로 바꾸는 것.

## useEffect()
```swift
useEffect(() => {
  ...
}, [deps])
```
1. 페이지가 처음 렌더링 되고 난 후 useEffect는 무조건 한 번 실행됩니다. [] <br>
2. useEffect에 배열로 지정한 useState의 값이 변경되면 실행되게 됩니다. <br>

즉, useEffect는 렌더링, 혹은 변수의 값 혹은 오브젝트가 달라지게 되면 그것을 인지하고 업데이트를 해주는 함수입니다.<br>
useEffect는 콜백 함수를 부르게 되며, 렌더링 혹은 값, 오브젝트의 변경에 따라 어떠한 함수 혹은 여러 개의 함수들을 <br>
동작시킬 수 있습니다.





