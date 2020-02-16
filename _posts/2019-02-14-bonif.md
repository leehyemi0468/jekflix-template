---
layout: post
title: "BONIF"
date: 2019-02-14 :00:0
image: 'https://leehyemi0468.github.io/assets/img/board_img/ryanfarm/index.bmp'
description: e-Commerce market web application.
category: 'team project'
tags:
- Spring
- jQuery
- Java 8
twitter_text: e-Commerce market web application.
introduction: e-Commerce market web application.
---

## Intro
>   

## Skills
1. 쿠폰관련

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

2. 주문관련

> ㄹㅇ

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
>  
-----

Want to see our project in Git? <a href="">Open an issue.</a>









