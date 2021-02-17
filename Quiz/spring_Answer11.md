# 1. 스프링 시큐리티에 대하여 설명하시오
- **인증과 권한**에 대한 프레임워크

# 2.스트링시큐리티를 적용하기 위한 기본 설정 및 세팅을 설명하시오
1. pom.xml에 **dependecy를 추가**해(총 4개) 라이브러리 다운 
    - 인증과 권한을 컨트롤하기 위함! 

2. **filter 설정** 
    - 위치는 반드시 한글필터 밑!

3. **root-context.xml 위치에 security-context.xml 생성**
```xml
<!--security 기본 설정-->
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/security"
    xmlns:beans="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">   
    
    <http>
        <form-login />
    </http>

    <authentication-manager></authentication-manager>

</beans:beans>
```
### - 설정 후 web.xml에 security-context.xml 추가
```xml
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/spring/root-context.xml
	/WEB-INF/spring/security-context.xml</param-value>
</context-param>
```
<br>

# 3.인증과 권한에 대하여 설명하시오
- 인증 : 나인지를 증명하는 것 
    - EX] 회원임을 인증 - 로그인(아이디, 비밀번호) / 회사원임을 인증 - 사원증으로 회사 출입 
- 권한 : 리소스 접근에 대한 자격을 부여하는 것 , admin과 일반유저인지에 따라 접근권한이 달라짐
    - EX] 회원 권한 - 글 삭제,쓰기, 읽기 등 / 회사내 부서 사무실 권한 - 한번 더 사원증으로 부서사람임을 확인

# 4.XSS 와 CSRF에 대하여 설명하시오
- **XSS** (Cross Site Scripting) : 게시판이나 웹 메일등에 **자바스크립트와 같은 스크립트 코드를 삽입해** 개발자가 **고려하지 않은 기능이 작동하게 하는 치명적인 공격** <br> 클라이언트(사용자)를 대상으로 한 공격
    >> [참고] CSS라고 불려야되지만 이미 CSS란 약자가 존재하기 때문에 XSS로 명칭

- **CSRF**(Cross-Site Request Forgery) : **클라이언트와의 의지와는 무관하게** 공격자가 의도한 행위를 **특정 웹사이트에 요청하게 하는 공격**

### XSS 와 CSRF의 차이는 공격대상!
- xss 공격 대상 : 클라이언트 / csrf 공격 대상 : Server

# 5. 부트스트랩 (eshopper)를 이용해 아래 조건을 만족해 프로그래밍해라
- 1 : N 관계인 emp , dept 조인해서 dname과 ename 출력
    >> [힌트] collection, resultMap
- 주어진 사진은 랜덤으로 처리할 것 
- uri/10 번 치면 10번에 해당하는 부서에 사원만 출력 10~ 30까지


## - Controller
```java
package edu.bit.ex.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import edu.bit.ex.service.EmpService;
import edu.bit.ex.vo.EmpVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class EmpShopController {
	
	private EmpService empService;
	
	@RequestMapping("/emplist")
	public String empListShop(Model model) {
		log.info("empListShop()실행");
		
		model.addAttribute("empList", empService.empList());
		
		return "/empShopBoot/empListShop";
		
	}
	
	@RequestMapping("/emplist/{deptno}")
	public String empListShopDeptno(EmpVO empVO, Model model) {
		log.info("empListShop10() 실행");
		
		model.addAttribute("empListDeptno", empService.empList10(empVO.getDeptno()));
		
		return "/empShopBoot/empListShopDeptno";
	}	
}
```

## - Service, ServiceImpl
```java
package edu.bit.ex.service;

import java.util.List;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

public interface EmpService {
	
	public List<EmpVO> empList();

	public List<EmpVO> empList10(int num);

}

package edu.bit.ex.service;

import java.util.List;

import org.springframework.stereotype.Service;

import edu.bit.ex.mapper.EmpMapper;
import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Service
public class EmpServiceImpl implements EmpService {
	
	private EmpMapper empMapper;
	
	@Override
	public List<EmpVO> empList() {
		
		return empMapper.empList();
	}

	@Override
	public List<EmpVO> empList10(int num) {
		
		return empMapper.getListDeptno(num);
	}
}
```

## - Mapper
```java
package edu.bit.ex.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

@Mapper
public interface EmpMapper {

	public List<EmpVO> empList();

	public List<EmpVO> getListDeptno(int num);
}
```

## - Mapper.xml
- 1:N관계에서 1에 해당하는 클래스(dept)에 N에 해당하는(empno) VO객체를 ArrayList형태로 추가해준다
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.EmpMapper">

<resultMap type="edu.bit.ex.vo.DeptVO" id="dept">
	<result column="DEPTNO" property="deptno"></result>
	<result column="DNAME" property="dname"></result>
	<result column="LOC" property="loc"></result>
</resultMap>

<resultMap type="edu.bit.ex.vo.EmpVO" id="empVO">
	<id property="empno" column="EMPNO" /> <!-- pk알려주는 용도 -->
	<result column="EMPNO" property="empno"/>
	<result column="ENAME" property="ename"/>
	<result column="JOB" property="job"/>
	<result column="MGR" property="mgr"/>
	<result column="HIREDATE" property="hiredate"/>
	<result column="SAL" property="sal"/>
	<result column="COMM" property="comm"/>
	<result column="DEPTNO" property="deptno"/>
	<collection property="deptVO" resultMap="dept"></collection>
</resultMap>

<select id="empList" resultMap="empVO"> 
<![CDATA[ 
select e.empno, e.ename, e.job, e.mgr, e.hiredate, e.sal, e.comm, e.deptno, d.dname, d.loc from emp e, dept d where e.deptno = d.deptno
]]>
</select>

<select id="getListDeptno" resultMap="empVO"> 
<![CDATA[ 
select e.empno, e.ename, e.job, e.mgr, e.hiredate, e.sal, e.comm, e.deptno, d.dname, d.loc from emp e, dept d where e.deptno = d.deptno and e.deptno in (select deptno from emp where deptno=#{num})
]]>
</select>
</mapper>
```

## - View 
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="">
    <meta name="author" content="">
    <title>Shop | E-Shopper</title>
    <link href="${pageContext.request.contextPath}/resources/eshopper/css/bootstrap.min.css" rel="stylesheet">
    <link href="${pageContext.request.contextPath}/resources/eshopper/css/font-awesome.min.css" rel="stylesheet">
    <link href="${pageContext.request.contextPath}/resources/eshopper/css/prettyPhoto.css" rel="stylesheet">
    <link href="${pageContext.request.contextPath}/resources/eshopper/css/price-range.css" rel="stylesheet">
    <link href="${pageContext.request.contextPath}/resources/eshopper/css/animate.css" rel="stylesheet">
	<link href="${pageContext.request.contextPath}/resources/eshopper/css/main.css" rel="stylesheet">
	<link href="${pageContext.request.contextPath}/resources/eshopper/css/responsive.css" rel="stylesheet">
    <!--[if lt IE 9]>
    <script src="js/html5shiv.js"></script>
    <script src="js/respond.min.js"></script>
    <![endif]-->       
    <link rel="shortcut icon" href="images/ico/favicon.ico">
    <link rel="apple-touch-icon-precomposed" sizes="144x144" href="images/ico/apple-touch-icon-144-precomposed.png">
    <link rel="apple-touch-icon-precomposed" sizes="114x114" href="images/ico/apple-touch-icon-114-precomposed.png">
    <link rel="apple-touch-icon-precomposed" sizes="72x72" href="images/ico/apple-touch-icon-72-precomposed.png">
    <link rel="apple-touch-icon-precomposed" href="images/ico/apple-touch-icon-57-precomposed.png">
</head><!--/head-->

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script type="text/javascript">

	$(document).ready(function() {
		var imgArray = new Array();

		imgArray[0] = "product7.jpg"
		imgArray[1] = "product8.jpg"
		imgArray[2] = "product9.jpg"
		imgArray[3] = "product10.jpg"
		imgArray[4] = "product11.jpg"
		imgArray[5] = "product12.jpg"

		var objImg = document.getElementsByClassName("showImg");
		
		for (var i = 0; i < objImg.length; i++) {
			var imgNum = Math.round(Math.random() * 5);
			// ${pageContext.request.contextPath}/resources/eshopper/images/shop/
			var src = objImg[i].src;
			//objImg[i].src = src + imgArray[imgNum];
			objImg[i].src += imgArray[imgNum];
		}
	});
</script>

<body>
	<header id="header"><!--header-->
		<div class="header_top"><!--header_top-->
			<div class="container">
				<div class="row">
					<div class="col-sm-6 ">
						<div class="contactinfo">
							<ul class="nav nav-pills">
								<li><a href=""><i class="fa fa-phone"></i> +2 95 01 88 821</a></li>
								<li><a href=""><i class="fa fa-envelope"></i> info@domain.com</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-6">
						<div class="social-icons pull-right">
							<ul class="nav navbar-nav">
								<li><a href=""><i class="fa fa-facebook"></i></a></li>
								<li><a href=""><i class="fa fa-twitter"></i></a></li>
								<li><a href=""><i class="fa fa-linkedin"></i></a></li>
								<li><a href=""><i class="fa fa-dribbble"></i></a></li>
								<li><a href=""><i class="fa fa-google-plus"></i></a></li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</div><!--/header_top-->
		
		<div class="header-middle"><!--header-middle-->
			<div class="container">
				<div class="row">
					<div class="col-md-4 clearfix">
						<div class="logo pull-left">
							<a href="${pageContext.request.contextPath}/resources/eshopper/index.html"><img src="${pageContext.request.contextPath}/resources/eshopper/images/home/logo.png" alt="" /></a>
						</div>
						<div class="btn-group pull-right clearfix">
							<div class="btn-group">
								<button type="button" class="btn btn-default dropdown-toggle usa" data-toggle="dropdown">
									USA
									<span class="caret"></span>
								</button>
								<ul class="dropdown-menu">
									<li><a href="">Canada</a></li>
									<li><a href="">UK</a></li>
								</ul>
							</div>
							
							<div class="btn-group">
								<button type="button" class="btn btn-default dropdown-toggle usa" data-toggle="dropdown">
									DOLLAR
									<span class="caret"></span>
								</button>
								<ul class="dropdown-menu">
									<li><a href="">Canadian Dollar</a></li>
									<li><a href="">Pound</a></li>
								</ul>
							</div>
						</div>
					</div>
					<div class="col-md-8 clearfix">
						<div class="shop-menu clearfix pull-right">
							<ul class="nav navbar-nav">
								<li><a href=""><i class="fa fa-user"></i> Account</a></li>
								<li><a href=""><i class="fa fa-star"></i> Wishlist</a></li>
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/checkout.html"><i class="fa fa-crosshairs"></i> Checkout</a></li>
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/cart.html"><i class="fa fa-shopping-cart"></i> Cart</a></li>
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/login.html"><i class="fa fa-lock"></i> Login</a></li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</div><!--/header-middle-->
	
		<div class="header-bottom"><!--header-bottom-->
			<div class="container">
				<div class="row">
					<div class="col-sm-9">
						<div class="navbar-header">
							<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
								<span class="sr-only">Toggle navigation</span>
								<span class="icon-bar"></span>
								<span class="icon-bar"></span>
								<span class="icon-bar"></span>
							</button>
						</div>
						<div class="mainmenu pull-left">
							<ul class="nav navbar-nav collapse navbar-collapse">
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/index.html">Home</a></li>
								<li class="dropdown"><a href="#" class="active">Shop<i class="fa fa-angle-down"></i></a>
                                    <ul role="menu" class="sub-menu">
                                        <li><a href="${pageContext.request.contextPath}/resources/eshopper/shop.html" class="active">Products</a></li>
										<li><a href="${pageContext.request.contextPath}/resources/eshopper/roduct-details.html">Product Details</a></li> 
										<li><a href="${pageContext.request.contextPath}/resources/eshopper/checkout.html">Checkout</a></li> 
										<li><a href="${pageContext.request.contextPath}/resources/eshopper/cart.html">Cart</a></li> 
										<li><a href="${pageContext.request.contextPath}/resources/eshopper/login.html">Login</a></li> 
                                    </ul>
                                </li> 
								<li class="dropdown"><a href="#">Blog<i class="fa fa-angle-down"></i></a>
                                    <ul role="menu" class="sub-menu">
                                        <li><a href="${pageContext.request.contextPath}/resources/eshopper/blog.html">Blog List</a></li>
										<li><a href="${pageContext.request.contextPath}/resources/eshopper/blog-single.html">Blog Single</a></li>
                                    </ul>
                                </li> 
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/404.html">404</a></li>
								<li><a href="${pageContext.request.contextPath}/resources/eshopper/contact-us.html">Contact</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-3">
						<div class="search_box pull-right">
							<input type="text" placeholder="Search"/>
						</div>
					</div>
				</div>
				</div>
			</div>
	</header>
	
	<section id="advertisement">
		<div class="container">
			<img src="${pageContext.request.contextPath}/resources/eshopper/images/shop/advertisement.jpg" alt="" />
		</div>
	</section>
	
	<section>
		<div class="container">
			<div class="row">
				<div class="col-sm-3">
					<div class="left-sidebar">
						<h2>Category</h2>
						<div class="panel-group category-products" id="accordian"><!--category-productsr-->
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title">
										<a data-toggle="collapse" data-parent="#accordian" href="#sportswear">
											<span class="badge pull-right"><i class="fa fa-plus"></i></span>
											Sportswear
										</a>
									</h4>
								</div>
								<div id="sportswear" class="panel-collapse collapse">
									<div class="panel-body">
										<ul>
											<li><a href="">Nike </a></li>
											<li><a href="">Under Armour </a></li>
											<li><a href="">Adidas </a></li>
											<li><a href="">Puma</a></li>
											<li><a href="">ASICS </a></li>
										</ul>
									</div>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title">
										<a data-toggle="collapse" data-parent="#accordian" href="#mens">
											<span class="badge pull-right"><i class="fa fa-plus"></i></span>
											Mens
										</a>
									</h4>
								</div>
								<div id="mens" class="panel-collapse collapse">
									<div class="panel-body">
										<ul>
											<li><a href="">Fendi</a></li>
											<li><a href="">Guess</a></li>
											<li><a href="">Valentino</a></li>
											<li><a href="">Dior</a></li>
											<li><a href="">Versace</a></li>
											<li><a href="">Armani</a></li>
											<li><a href="">Prada</a></li>
											<li><a href="">Dolce and Gabbana</a></li>
											<li><a href="">Chanel</a></li>
											<li><a href="">Gucci</a></li>
										</ul>
									</div>
								</div>
							</div>
							
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title">
										<a data-toggle="collapse" data-parent="#accordian" href="#womens">
											<span class="badge pull-right"><i class="fa fa-plus"></i></span>
											Womens
										</a>
									</h4>
								</div>
								<div id="womens" class="panel-collapse collapse">
									<div class="panel-body">
										<ul>
											<li><a href="">Fendi</a></li>
											<li><a href="">Guess</a></li>
											<li><a href="">Valentino</a></li>
											<li><a href="">Dior</a></li>
											<li><a href="">Versace</a></li>
										</ul>
									</div>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Kids</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Fashion</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Households</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Interiors</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Clothing</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Bags</a></h4>
								</div>
							</div>
							<div class="panel panel-default">
								<div class="panel-heading">
									<h4 class="panel-title"><a href="#">Shoes</a></h4>
								</div>
							</div>
						</div><!--/category-productsr-->
					
						<div class="brands_products"><!--brands_products-->
							<h2>Brands</h2>
							<div class="brands-name">
								<ul class="nav nav-pills nav-stacked">
									<li><a href=""> <span class="pull-right">(50)</span>Acne</a></li>
									<li><a href=""> <span class="pull-right">(56)</span>GrÃ¼ne Erde</a></li>
									<li><a href=""> <span class="pull-right">(27)</span>Albiro</a></li>
									<li><a href=""> <span class="pull-right">(32)</span>Ronhill</a></li>
									<li><a href=""> <span class="pull-right">(5)</span>Oddmolly</a></li>
									<li><a href=""> <span class="pull-right">(9)</span>Boudestijn</a></li>
									<li><a href=""> <span class="pull-right">(4)</span>RÃ¶sch creative culture</a></li>
								</ul>
							</div>
						</div><!--/brands_products-->
						
						<div class="price-range"><!--price-range-->
							<h2>Price Range</h2>
							<div class="well">
								 <input type="text" class="span2" value="" data-slider-min="0" data-slider-max="600" data-slider-step="5" data-slider-value="[250,450]" id="sl2" ><br />
								 <b>$ 0</b> <b class="pull-right">$ 600</b>
							</div>
						</div><!--/price-range-->
						
						<div class="shipping text-center"><!--shipping-->
							<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/shipping.jpg" alt="" />
						</div><!--/shipping-->
						
					</div>
				</div>
				
				<div class="col-sm-9 padding-right">
					<div class="features_items"><!--features_items-->
						<h2 class="title text-center">Features Items</h2>
						
						<c:forEach items="${empList}" var="dao">
						<div class="col-sm-4">
							<div class="product-image-wrapper">
								<div class="single-products">
										<div class="productinfo text-center">
											<img class="showImg" src="${pageContext.request.contextPath}/resources/eshopper/images/shop/" alt="" />
											
											<c:forEach items="${dao.deptVO}" var="deptItem"> <!-- 객체안에 List가 있기 때문에 원하는 변수의 값을 얻기위해서는 forEach문 또 돌려야함 -->
												<h2>${deptItem.dname}</h2>
											</c:forEach>
											<p>${dao.ename}</p>
											<a href="#" class="btn btn-default add-to-cart"><i
												class="fa fa-shopping-cart"></i>Add to cart</a>
										</div>
										<div class="product-overlay">
											<div class="overlay-content">

												<c:forEach items="${dao.deptVO}" var="deptItem">
													<h2>${deptItem.dname}</h2>
												</c:forEach>
												<p>${dao.ename}</p>
												<a href="#" class="btn btn-default add-to-cart"><i
													class="fa fa-shopping-cart"></i>Add to cart</a>
											</div>
										</div>
									</div>
								<div class="choose">
									<ul class="nav nav-pills nav-justified">
										<li><a href=""><i class="fa fa-plus-square"></i>Add to wishlist</a></li>
										<li><a href=""><i class="fa fa-plus-square"></i>Add to compare</a></li>
									</ul>
								</div>
							</div>
						</div>
						
					
						</c:forEach>
						<ul class="pagination">
							<li class="active"><a href="">1</a></li>
							<li><a href="">2</a></li>
							<li><a href="">3</a></li>
							<li><a href="">&raquo;</a></li>
						</ul>
					</div><!--features_items-->
				</div>
			</div>
		</div>
	</section>
	
	<footer id="footer"><!--Footer-->
		<div class="footer-top">
			<div class="container">
				<div class="row">
					<div class="col-sm-2">
						<div class="companyinfo">
							<h2><span>e</span>-shopper</h2>
							<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit,sed do eiusmod tempor</p>
						</div>
					</div>
					<div class="col-sm-7">
						<div class="col-sm-3">
							<div class="video-gallery text-center">
								<a href="#">
									<div class="iframe-img">
										<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/iframe1.png" alt="" />
									</div>
									<div class="overlay-icon">
										<i class="fa fa-play-circle-o"></i>
									</div>
								</a>
								<p>Circle of Hands</p>
								<h2>24 DEC 2014</h2>
							</div>
						</div>
						
						<div class="col-sm-3">
							<div class="video-gallery text-center">
								<a href="#">
									<div class="iframe-img">
										<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/iframe2.png" alt="" />
									</div>
									<div class="overlay-icon">
										<i class="fa fa-play-circle-o"></i>
									</div>
								</a>
								<p>Circle of Hands</p>
								<h2>24 DEC 2014</h2>
							</div>
						</div>
						
						<div class="col-sm-3">
							<div class="video-gallery text-center">
								<a href="#">
									<div class="iframe-img">
										<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/iframe3.png" alt="" />
									</div>
									<div class="overlay-icon">
										<i class="fa fa-play-circle-o"></i>
									</div>
								</a>
								<p>Circle of Hands</p>
								<h2>24 DEC 2014</h2>
							</div>
						</div>
						
						<div class="col-sm-3">
							<div class="video-gallery text-center">
								<a href="#">
									<div class="iframe-img">
										<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/iframe4.png" alt="" />
									</div>
									<div class="overlay-icon">
										<i class="fa fa-play-circle-o"></i>
									</div>
								</a>
								<p>Circle of Hands</p>
								<h2>24 DEC 2014</h2>
							</div>
						</div>
					</div>
					<div class="col-sm-3">
						<div class="address">
							<img src="${pageContext.request.contextPath}/resources/eshopper/images/home/map.png" alt="" />
							<p>505 S Atlantic Ave Virginia Beach, VA(Virginia)</p>
						</div>
					</div>
				</div>
			</div>
		</div>
		
		<div class="footer-widget">
			<div class="container">
				<div class="row">
					<div class="col-sm-2">
						<div class="single-widget">
							<h2>Service</h2>
							<ul class="nav nav-pills nav-stacked">
								<li><a href="">Online Help</a></li>
								<li><a href="">Contact Us</a></li>
								<li><a href="">Order Status</a></li>
								<li><a href="">Change Location</a></li>
								<li><a href="">FAQâs</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-2">
						<div class="single-widget">
							<h2>Quock Shop</h2>
							<ul class="nav nav-pills nav-stacked">
								<li><a href="">T-Shirt</a></li>
								<li><a href="">Mens</a></li>
								<li><a href="">Womens</a></li>
								<li><a href="">Gift Cards</a></li>
								<li><a href="">Shoes</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-2">
						<div class="single-widget">
							<h2>Policies</h2>
							<ul class="nav nav-pills nav-stacked">
								<li><a href="">Terms of Use</a></li>
								<li><a href="">Privecy Policy</a></li>
								<li><a href="">Refund Policy</a></li>
								<li><a href="">Billing System</a></li>
								<li><a href="">Ticket System</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-2">
						<div class="single-widget">
							<h2>About Shopper</h2>
							<ul class="nav nav-pills nav-stacked">
								<li><a href="">Company Information</a></li>
								<li><a href="">Careers</a></li>
								<li><a href="">Store Location</a></li>
								<li><a href="">Affillate Program</a></li>
								<li><a href="">Copyright</a></li>
							</ul>
						</div>
					</div>
					<div class="col-sm-3 col-sm-offset-1">
						<div class="single-widget">
							<h2>About Shopper</h2>
							<form action="#" class="searchform">
								<input type="text" placeholder="Your email address" />
								<button type="submit" class="btn btn-default"><i class="fa fa-arrow-circle-o-right"></i></button>
								<p>Get the most recent updates from <br />our site and be updated your self...</p>
							</form>
						</div>
					</div>
					
				</div>
			</div>
		</div>
		
		<div class="footer-bottom">
			<div class="container">
				<div class="row">
					<p class="pull-left">Copyright Â© 2013 E-Shopper. All rights reserved.</p>
					<p class="pull-right">Designed by <span><a target="_blank" href="http://www.themeum.com">Themeum</a></span></p>
				</div>
			</div>
		</div>
		
	</footer><!--/Footer-->
	

  
    <script src="${pageContext.request.contextPath}/resources/eshopper/js/jquery.js"></script>
	<script src="${pageContext.request.contextPath}/resources/eshopper/js/price-range.js"></script>
    <script src="${pageContext.request.contextPath}/resources/eshopper/js/jquery.scrollUp.min.js"></script>
	<script src="${pageContext.request.contextPath}/resources/eshopper/js/bootstrap.min.js"></script>
    <script src="${pageContext.request.contextPath}/resources/eshopper/js/jquery.prettyPhoto.js"></script>
    <script src="${pageContext.request.contextPath}/resources/eshopper/js/main.js"></script>
</body>
</html>
```

### ▶ 출력 
![empshopper1](https://user-images.githubusercontent.com/74290204/108077381-cf091a00-70af-11eb-9f79-b10cabba975a.png)
![화면 캡처 2021-02-16 225748](https://user-images.githubusercontent.com/74290204/108077391-d16b7400-70af-11eb-81a0-cab311a0c9ce.png)

## *선생님 정답*
### 1. Controller
- deptno를 따로 나눠서 맵핑하지 않음!

```java
@GetMapping("/list2/{deptNo}")
	public String list2Emp(@PathVariable("deptNo") int deptno, Model model) {

		// 10 20 30...
		DeptEmpVO deptEmp = empService.selectEmpDeptName(deptno);
		System.out.println(deptEmp);		
		//model.addAttribute("emps", deptEmp.getEmpList());
		
		model.addAttribute("deptEmps", deptEmp);

		return "emp2";
	}
```

### 2. Service
```java
public DeptEmpVO selectEmpDeptName(int deptno) {
		return empMapper.selectEmpDeptName(deptno);
	}
```

### 3. Mapper
- 부모쪽에서 자식쪽을 List로 받아낸다
>> [1:N관계에서의 sql 처리 정리 참조] https://github.com/seulpi/TIL/blob/main/Basic/DB/DB_basic03(Join).md

```java
public interface EmpMapper {

	DeptEmpVO selectEmpDeptName(int deptno);
}
```

### 4. Mapeer.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="edu.bit.emp.mapper.EmpMapper">

	<resultMap id="Emp" type="edu.bit.emp.vo.EmpVO">
	    <id property="empno" column="empno"/>
	    <result property="ename" column="ename"/>
	    <result property="job" column="job"/>
	</resultMap>
	
	<resultMap id="DeptEmpResult" type="edu.bit.emp.vo.DeptEmpVO" >
		<id property="deptno" column="deptno" />
		<result property="dname" column="dname"/>
		<result property="loc" column="loc" />
		<collection property="empList" javaType="java.util.ArrayList" resultMap="Emp" />
	</resultMap>
	
	<select id="selectEmpDeptName" parameterType="int" resultMap="DeptEmpResult">
	<![CDATA[
		select  * from emp e ,dept d where e.deptno = d.deptno and d.deptno = #{deptno}	
  	]]>
	</select>

</mapper>
```

### 5. view
- 이미지랜덤 돌리는 것 → "~product" + Math~ + ".jag" 다이렉트로 url 때려버림(어차피 사진뒤에 숫자만 바뀌면 되니까! 놀랍..나는 왜 생각못했지..배열 사용하는 거보다 훨씬 간단하고 쉽다)

```jsp
<script  src="http://code.jquery.com/jquery-latest.min.js"></script>
<script type="text/javascript">
	$(function(){
		$('.features_img').each(function (index, item) {	        	
	        	var imgUrl = '${pageContext.request.contextPath}/resources/images/home/product' + Math.floor((Math.random() * 6) + 1) + '.jpg';
	        	console.log(imgUrl);
	        	console.log($(item).attr("src"));
	        	$(item).attr("src", imgUrl);
	        });

	});
</script>

<body>
	<div class="col-sm-9 padding-right">
		<div class="features_items"><!--features_items-->
			<h2 class="title text-center">Features Items</h2>
			<!-- ================================================= -->
			<div class="features_items"><!--features_items-->
			<c:set var="deptEmps" value="${deptEmps}" />
						
			<c:forEach var="emp" items="${deptEmps.empList}" varStatus="status">
				<div class="col-sm-4">
					<div class="product-image-wrapper">
						<div class="single-products">
							<div class="productinfo text-center">
								<img class="features_img" src="${pageContext.request.contextPath}/resources/images/home/product6.jpg" alt="" />
								<h2>${deptEmps.dname}</h2>
								<p>${emp.ename}</p>
								<a href="#" class="btn btn-default add-to-cart"><i class="fa fa-shopping-cart"></i>Add to cart</a>
							</div>
											
							<div class="product-overlay" id="poverlay">
								<div class="overlay-content">
								<h2><fmt:setLocale value="ko_KR"/><fmt:formatNumber type="currency" value="${emp.sal}" /></h2>
								<p>${emp.ename}</p>
								<a href="#" class="btn btn-default add-to-cart"><i class="fa fa-shopping-cart"></i>Add to cart</a>
								</div>
							</div>											
						</div>
						
						<div class="choose">
							<ul class="nav nav-pills nav-justified">
								<li><a href="#"><i class="fa fa-plus-square"></i>Add to wishlist</a></li>
								<li><a href="#"><i class="fa fa-plus-square"></i>Add to compare</a></li>
							</ul>
						</div>
					</div>
				</div>
				</c:forEach>						
			</div><!--features_items-->
</body>

