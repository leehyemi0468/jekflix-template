---
layout: post
title: "RyanFarm"
date: 2017-09-10 12:26:40
image: 'https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/index.bmp'
description: e-Commerce market web application.
category: 'team project'
tags:
- Spring
- jQuery
twitter_text: e-Commerce market web application.
introduction: e-Commerce market web application.
---

## Intro
>   2017년 7월, 저렴하고 영양도 풍부하여 식탁에 거의 빠질 일이 없는 계란의 살충제파동은 우리에게 큰 충격을 주었다. 먹거리안전에 대해 다시 생각해 보다가 이런 웹서비스는 없을까 해서 **RyanFarm**을 개발하게 되었다. 기존 농산물 전자상거래사이트는 많이 존재한다. 옥션이나 지마켓같은 대형마켓은 판매자와 구매자의 거래가 이루어지지만 판매자의 정보를 믿을 수 없다는 단점이 있고 일반 농산물 거래사이트는 개인사업자가 직접올리고 약간의 인증절차가 있기 때문에 믿을 수는 있지만 개인간의 거래는 이루어 질 수 없는 단점이 있다. **RyanFarm**은 이러한 두 사이트 SWOT기법을 적용하여 C2C, B2C는 물론 인증을 걸친 사업자의 상품을 **RyanFarm**이 직접 올리고 관리,인증하는 서비스를 제공하는 웹싸이트를 개발하였다. 


## Skills
1. OAuth를 이용한 회원가입

> 일반회원계정과 SNS연동회원계정 2가지 login절차로 나누어져 있는데 DataBase에는 한 테이블로 두 계정을 관리한다. 
 
 * 구현화면 
 
  ![placeholder](https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/loginform.bmp "Small example image")
 
 * MemberTable
<table>
  <thead>
    <tr> <th>필수여부</th>
      <th>Userid</th><th>Passwd</th><th>Username</th><th>...</th><th>jointype</th><th>sns_id</th>
    </tr>
  </thead>
 <tbody>
  <tr><td>일반계정</td><td>○</td><td>○</td><td>○</td><td>...</td><td>○</td><td>○</td></tr>
   <tr><td>SNS연동계정</td><td>Email사용</td><td></td><td>받아옴</td><td>...</td><td>일반계정과 구분</td><td>연동사이트에서 받은 고유키값
</td></tr>
 </tbody>
</table>

 > SNS연동회원의 가입시 최소정보만 기입하도록 구현하고 필요할 때마다 사용자에게 작성하도록 유도한다.

* 주요 Code

```js
// 1. JS로 Google SDK 끌어오기
<meta name="google-signin-scope" content="profile email">
<meta name="google-signin-client_id" content="83117513079-2k0pf4h20vdph70qps0mi8a8l1d9k9h1.apps.googleusercontent.com">
<script src="https://apis.google.com/js/platform.js" async defer></script>

// 2. Google - Login Button 생성하기
		<div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark"></div>
		
//3.  callback 함수
   <script>
      function onSignIn(googleUser) {
        var profile = googleUser.getBasicProfile();
        location.href = "url/맵핑값?state=".concat(googleUser.getAuthResponse().id_token,"&저장할 키값=",벨류...);
       };
    </script>  
```

2. Google - Recaptcha

> 금전적인 거래가 이루어지는 사이트이다보니 보안을 생각하여 가입자동프로그램을 사용하는 봇을 필터링해주는 captcha기술을 일반계정 회원가입란에 사용하였다. View에서 봇인지 아닌지 검증을 하고 구글에서 받은 리턴값을 Controller에서 검사한다. 겉으로 보기엔 일반 체크박스와 다를 것이 없지만 이미 구글의 패턴분석으로 검증한 결과인 것. 만약 같은 IP에서 여러번 가입을 하는 등의 이상한 움직임을 recaptcha가 발견하면 검증단계를 한번 더 거치게 된다.

* 구현화면 

 ![placeholder](https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/joinform.bmp "Small example image")

* 주요 Code

```java

	 URL url = new URL("https://www.google.com/recaptcha/api/siteverify?secret=시크릿키&response="+응답값);
         HttpURLConnection conn = (HttpURLConnection) url.openConnection();
         ...header 설정...

	//출력
	DataOutputStream wr = new DataOutputStream(conn.getOutputStream());
	BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
	String inputLine;
	StringBuffer response = new StringBuffer();
	while ((inputLine = in.readLine()) != null) {
		response.append(inputLine);
	}
	
	//String을 Json으로 Parsing
	JsonReader jsonReader = Json.createReader(new StringReader(response.toString()));
	JsonObject jsonObject = jsonReader.readObject();
	if(jsonObject.getBoolean("success") == true) {
		Insert Member...
	}
			
```



3. Q&A Board Paging, 1-Depth Replying  처리

>  로그인한 사용자가 쓴 질문글과 Admin이 쓴 답변글만 볼 수 있도록 구현이 되었다.  또 답변을 달 기 전까진 문의글을 수정, 삭제 할 수 있지만 답변이 달린 뒤에는 수정,삭제를 하지 못하게 구현했다. 관리자의 편의와 빠른 답변처리를 위해 댓글은 1번까지만 달 수 있도록 구현했고 사용자가 추가질문을 원하면 새 문의글을 다시 등록해야 한다.

* 구현화면 

 ![placeholder](https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/paging_reply.jpg "Small example image")

* QnaTable
<table>
  <thead>
    <tr>
      <th>Qna_No</th><th>Group_No</th><th>Status</th>
    </tr>
  </thead>
 <tbody>
  <tr><td>문의,답변글 구분없이 전부 가져야 하는 고유값</td><td>질문,답변글 묶기</td><td> 처리중,답변완료 구분</td></tr>
 </tbody>
</table>
 > Q&A Table에 Group_No Column로 질문,답변글 묶기 / Status Column로 처리중,답변완료 구분을 하였고 List나열시 order by qna_no , group_no로 정렬을 해주었다.


* 주요 Code
```js
<c:set var="curPage" value="${qnaList.curPage}" />
<c:set var="perPage" value="${QnaPageDTO.getPerPage()}" />
<c:set var="totalCount" value="${qnaList.totalCount}" />
<fmt:parseNumber var="totalNum" integerOnly="true"	value="${qnaList.getList().size()/perPage}" />
<c:if test="${totalCount%perPage!=0}">
<c:set var="totalNum" value="${totalNum+1}"/>
</c:if>
<c:if test="${startPage <1 }">
	<c:set var="startPage" value="1" />
</c:if>

<!-- totalNum == 총 페이지수, totalCount = 총 레코드 갯수 -->
<!-- 현재 페이지번호의 블럭번호 구하기 -->	
<fmt:parseNumber var="curBlock" integerOnly="true" value="${(curPage/perPage)+1 }" />
<!-- 시작페이지번호 구하기  -->	
<fmt:parseNumber var="startPage" integerOnly="true"	value="${(curBlock - 1)*perPage+1}" />
<!-- 마지막페이지번호 구하기 -->	
<fmt:parseNumber var="endPage" integerOnly="true" value="${(startPage +perPage)-1 }" />
<c:if test="${endPage > totalNum }">
	<c:set var="endPage" value="${totalNum}" />
</c:if>
<c:forEach begin="${startPage}" end="${totalNum}" varStatus="status">
<c:if test="${curPage==status.index}">
${status.index }
</c:if>
<c:if test="${curPage!=status.index }">
<a href="QNAList?userid=${sessionScope.login.userid}&curPage=${status.index}">${status.index}</a>
</c:if>
</c:forEach>
```


4. Mailing을 통한 비밀번호갱신

>  초반에는 비밀번호를 보여주는 식으로 구현하였지만 여러홈페이지마다 한 비밀번호를 쓰는 유저들도 있기 때문에 메일로 인증을 하고 인증된 사용자에게는 새 비밀번호를 갱신하는 방향으로 바꾸었다. server에서 random number 6자리를 생성하여 메일로 보내주고 view에서 사용자가 적은 번호와 server에서 생성한 난수가 일치하는지 체크한다.  SNS연동사용자는 애초에 비밀번호를 사용하지 않기 때문에 이 서비스가 필요없다.


* 구현화면 

 ![placeholder](https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/findPw_mail.bmp "Small example image")

* 주요 Code
```java
	private void sendEMail(String email, String authNum) {
		String host = "smtp.gmail.com";
		String fromName="RyanFarm";
		
		String content="인증번호 [" + authNum + "]";
		try {
			Properties props=new Properties();
			props.put("mail.smtp.starttls.enable", "true");
				...설정...
			props.setProperty("mail.smtp.soketFactory.class", "javax.net.ssl.SSLSocketFactory");
			
			Session session=Session.getInstance(props, new javax.mail.Authenticator() {
				protected PasswordAuthentication getPasswordAuthentication() {
					return new javax.mail.PasswordAuthentication("leehyemi0468@gmail.com", "비밀번호");	
				}
			});
			
			Message msg=new MimeMessage(session);
			msg.setFrom(new InternetAddress(from,MimeUtility.encodeText(
					fromName,"UTF-8","B")));
			InternetAddress[] address1= { new InternetAddress(to1)};
			msg.setRecipients(Message.RecipientType.TO, address1);
			...
			Transport.send(msg);
		}
	}	
```


## Outro
>  동적서비스를 해주는 e-commerce싸이트들의 기본기능들은 어느정도 구현된 것 같다. 여기에 빅데이터를 공부해서 적용하면 사용자별로 맞춤 검색서비스를 제공하여 검색결과들을 글자만 같은 결과들을 전부 보여주는 것이 아니라 한번 더 필터링 해주거나,  대기업 상거래싸이트에서 많이 볼 수 있는 검색기록들과 관련있는 광고들을 보여주는 등 완성도높은 웹사이트를 만들 수 있을 것 이다. UI/UX 를 더 수정하고 현재 SMS API를 제공해주던 곳이 서비스를 중단한 상태라 다른 API를 사용하는 방향으로 보안해야 할 것 같다. Git Team 사용이 처음이라 conflict Error가 자꾸 나서 서로 push/pull 할 때 초반에 애를 먹었다. 하지만 서로 Part를 나누어 각자의 맡은부분에 좀 더 집중하여 완성도를 높이는 장점도 발견할 수 있는 경험이었다. 

-----

Want to see our project in Git? <a href="https://github.com/kyungso/Farm_Spring">Open an issue.</a>









