---
layout: post
title: "RyanFarm"
date: 2017-09-10 12:26:40
image: 'https://leehyemi0468.github.io/assets/img/index.bmp'
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
<dd>1. OAuth를 이용한 회원가입 </dd>

 <dt>일반회원계정과 SNS연동회원계정 2가지 login절차로 나누어져 있는데 DataBase에는 한 테이블로 두 계정을 관리한다. </dt>
 
 <dt>* 구현화면 </dt>
 
  <dt>![placeholder](https://leehyemi0468.github.io/assets/img/loginform.bmp "Midium example image")</dt>
 
 * DB의 MemberTable
 SNS연동회원의  ID는 이메일주소이며 최소정보만 기입하도록 구현한다. 
<table>
  <thead>
    <tr> <th>필수여부</th>
      <th>Userid</th><th>Passwd</th><th>Username</th><th>Addr</th><th>Phone</th><th>Email</th><th>Joinday</th><th>jointype</th><th>sns_id</th><th>isSeller</th>
    </tr>
  </thead>
 <tbody>
  <tr><td>일반계정</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td><td>○</td></tr>
   <tr><td>SNS연동계정</td><td>Email=ID로 사용</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td>
  </tr>
 </tbody>
</table>



2 Google - Recaptcha



* 구현화면 

 ![placeholder](https://leehyemi0468.github.io/assets/img/joinform.bmp "Midium example image")





## Outro
>  보완점

-----

Want to see our project in Git? <a href="https://github.com/kyungso/Farm_Spring">Open an issue.</a>









