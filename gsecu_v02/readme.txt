#gsecu_v0.2

1. 개요.
	gsecu_v0.2는 Spring Security가 제공하는 기본 로그인 화면을
	개발자가 원하는대로 커스터마이징 한다.

2. 목표.
	1) 로그인 화면 커스터마이징.
      
3. 주요내용.
	3.1. 로그인 화면 커스터마이징.
		1) security-context.xml
			: 로그인 화면을 커스터마이징하기 위해서는 security-context.xml에 만들어져 있던 
			<http> 태그 안에 <form-login> 태그를 추가하고 커스터마이징 설정을 해야한다.
			
			<form-login 
			login-page="/login" default-target-url="/home" 
			username-parameter="userid" password-parameter="userpw" 
			authentication-failure-url="/login?fail=true"/>
			
			<!-- 
			# form-login 항목.
				1) always-use-default-target
					: 로그인이 성공 후 무조건 default-target-url 페이지로 이동하는 설정.
					true, false로 값을 설정한다.
					(기본 true, false로 값을 지정할 때 설정한다.)
					만약, 로그인이 성공하면 이전 로그인 접근 페이지나 특정 페이지를
					사용자에게 제공하려면 false로 설정하고 
					authentication-success-handler-ref 에 핸들러를 만들고 
					핸들러에서 이 부분을 구현해야 한다.
					예를 들자면, 관리자 로그인 성공시 각각의 관리자의 역할별로 
					각자 다른 인증 성공 페이지를 제공해야 할 때 등.
					
				2) authentication-details-source-ref
				
				3) authentication-failure-forward-url
				
				4) authentication-failure-handler-ref
					: 로그인 실패 후 부가작업을 진행할 때 담당할 Handler를 설정.
					(로그인 실패 핸들러는 로그인 실패 후 실패 카운트를 저장한다든가, 
					로그인 실패 로그를 쌓는다든가의 일을 할 수 있겠다.)
					만약, 이 핸들러를 적용하게 되면 
					authentication-failure-url 속성은 무시되며
					핸들러에서 직접 이동할 페이지를 지정해야 한다.
					
				5) authentication-failure-url
					: 로그인 실패 후 이동할 URL 설정.
					
				6) authentication-success-forward-url
				
				7) authentication-success-handler-ref
					: 로그인 성공 후 부가작업을 진행할 때 담당할 Handler를 설정.
					(로그인 성공 핸들러는 로그인 성공 후 방문자 수를 추가한다든가, 
					로그인 한 사람의 로그인 기록을 남긴다든가의 일을 할 수 있겠다.)
					만약, 이 핸들러를 적용하게 되면
					default-target-url 속성은 무시되며
					핸들러에서 직접 이동할 페이지를 지정해야 한다.
					
				8) default-target-url
					: 로그인 성공 후 이동할 URL 설정.
					
				9) login-page
					: 로그인 페이지의 주소를 커스터마이징 할 때 설정.
					
				10) login-processing-url
					: 로그인 페이지의 form action 주소를 커스터마이징 할 때 설정.
					그러나, 이 부분은 보여지는 부분일 뿐 실제로 동작하지 않는다.
					실제로는 'j_spring_security_check'로 호출되어 로그인을 처리한다.
					
				11) username-parameter
					: 로그인 페이지 id 이름을 커스터마이징 할 때 설정. 기본 이름은 'j_username'이다.
					커스터마이징 예)
					<input type="text" name="userid">
					
				12) password-parameter
					: 로그인 페이지 password 이름을 커스터마이징 할 때 설정. 기본 이름은 'j_password'이다.
					커스터마이징 예)
					<input type="password" name="userpw">
			-->

		2) 로그인 페이지(login.jsp) 만들기
			: WEB-INF/views/auth/login.jsp 를 생성.
			
			<form name="loginForm" method="POST">
				<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
				<table style="margin-left:auto; margin-right:auto;">
					<tr>
						<td>User:</td>
						<td><input type="text" name="userid"></td>
					</tr>
					<tr>
					<td>Password:</td>
						<td><input type="password" name="userpw"></td>
					</tr>
					<tr>
						<td></td>
						<td>
							<input name="submit" type="submit" value="Login" />
						</td>
					</tr>
				</table>
			</form>
		
		3) 로그인 성공 기본 페이지(home.jsp) 만들기
			: WEB-INF/views/home.jsp 를 생성.
		
4. 추가내용.
	4.1. 새로운 URL 추가.
		1) home
			: 로그인을 성공하면 기본으로 보여지는 페이지.
		
		2) goodbye
			: 로그아웃을 성공하면 기본으로 보여지는 페이지.
			
	4.2. JSTL의 core 태그 사용.(login.jsp)
		1) <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
