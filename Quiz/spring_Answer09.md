# 1. emp list 페이징을 구현하시오
## - dummy table 만들어서 테스트해 볼 것! (밑에 방법 제시한것도 지금 테스트 안해봐서 돌려봐야함)
```sql
--방법1 
-- dummy data 생성
begin
  for i in 1..1000 loop
   insert into emp (empno, ename, job, mgr, hiredate,sal,comm,deptno) values (ex_seq.nextval, "test" , "job" , 0, sysdate, 0, 0, 0);
  end loop;
end; --random으로 deptno을 쿼리로 줘야하는데 못 줄 때 spring test로 끌고와서 돌려본다 

create sequence ex_seq
increment by 1
start with 9091;
-- 시작 숫자 1

-- 방법2
begin
  for i in 1..1000 loop
   insert into emp (select max(empno) + 1 from emp, ename, job, mgr, hiredate,sal,comm,deptno) values (ex_seq.nextval, "test" , "job" , 0, sysdate, 0, 0, 0);
  end loop;
end;

-- emp 복사가 안되는데 그 이유는 이미 이름이 존재하는 key이름이어서 안됨 따라서 alter문을 통해서 forien key와 pk 를 따로 지정해줘야함 
CONSTRAINT "FK_DEPTNO" FOREIGN KEY ("DEPTNO") → 이 부분 , 우클릭/ 편집 / 제약조건 / + 새 제약조건 key 설정 
→ 이게 싫으면 그냥 다이렉트로해서 emp에 집어넣어도 나중에 @??뭐시기로 되돌릴수있다함
```
```java
//Creteria
package edu.bit.ex.page;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class Criteria {
	
	private int page; 
	private int amount;
	
	public Criteria() {
		this(1, 10); // 한 페이지당 10개씩 보여줄거야 
		
	}
	
	public Criteria(int page, int amount) {
		this.page = page;
		this.amount = amount;
	}
	

}
````
```java
//PageVO
package edu.bit.ex.page;

import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class PageVO {
	
	// 필요한 변수 페이지 시작점, 끝점, 이전, 다음, 총 페이지 수, 페이지&양 만들어놓은 객체
	private int startPage;
	private int endPage;
	private boolean next, previus;
	
	private int total;
	private Criteria cri;
	
	public PageVO(Criteria cri, int total) {
		this.cri = cri;
		this.total = total;
		
		// 이 생성자에서 strtPage와 endPage 등 변수에 대한 초기화, 정의
		this.endPage = (int)(Math.ceil(cri.getPage() /10.0)) * 10; // endPage는 10개로 보여줄거니까 들어온 페이지 값을 10개로 나눈 것에 대해 *10 
		this.startPage = this.endPage - 9;
		
		int realEnd = (int)(Math.ceil(total*1.0)/cri.getAmount()); 
    /* 1.0을 해주는 이유? ceil함수를 사용하기 위해 소수점으로 바꿔주기 위해서 , 
    realEnd는 게시글의 총갯수에 대한 계산이 아니라 80개라고 하면 총 8페이지여야하는데 10개로 우리가 PageVO에서 지정해줬으니까 10으로 end가 되기 때문에 8페이지로 나타내기 위한 계산  */
		
		if(realEnd <= this.endPage) {
			this.endPage = realEnd;
		}
		
		this.previus = this.startPage > 1; // 1보다 커야하는 이유는 1보다 커야 이전으로 그 전 목록으로 넘어갈수 있기 때문에 1이 페이징의 첫 시작임
		this.next = this.endPage < realEnd; // 그렇다면 endPage는 realPage보다 작아야 다음으로 갈 목록이 있는 것임
	}
	
	public String makeQuery(int page) { //1page, 2page,.. 값만으로 query문 계산해서 넘기기때문에 필요한 요소는 page값뿐임(rownum사용할거니까)
	
											// pageNum=3           pageNum=3&amount=10	build()→ ?pageNum=3&amount=10
		UriComponents uricomp = UriComponentsBuilder.newInstance().queryParam("pageNum", page).queryParam("amount", cri.getAmount()).build();
		
		/* UriComponents: 문자열 URI를 만들어야 할때, URI를 동적으로 만들어주는 클래스
		 * UriComponentsBuilder : 여러 개의 파라미터들을 연결하여 URL 형태로 만들어 주는 기능, 
		 * 즉 Controller단에서 addAttribute로 하나 하나 속성을 지정해주지 않아도 이 class를 이용하면 손쉽고 간단하게 파라미터 전달 가능*/
		 
		return uricomp.toString();
	}
}
```
```java
//controller
package edu.bit.ex.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import edu.bit.ex.page.Criteria;
import edu.bit.ex.page.PageVO;
import edu.bit.ex.service.EmpService;
import edu.bit.ex.vo.EmpVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;

@Log4j
@AllArgsConstructor
@Controller
public class EmpController {
	
	private EmpService empService;
	
	@RequestMapping("/list")
	public String empList(Criteria cri, Model model) {
		log.info("empList()실행");
		log.info("cri 실행" + cri);
		
		model.addAttribute("empList", empService.empList());
		model.addAttribute("empPage", empService.empPage(cri));
		int total = empService.getTotal(cri);
		
		log.info("total 실행" + total);
		model.addAttribute("pageMaker", new PageVO(cri, total));
		
		return "list";
		
	}
	
	@GetMapping("/EmpAdd_View")
	public String empAdd_View(Model model) {
		log.info("EmpAdd_View()실행");
		model.addAttribute("mgrList", empService.mgrList());
		model.addAttribute("dnameList", empService.dnameList());
		
		return "EmpAdd_View";
		
	}
	
	@PostMapping("/empInsert")
	public String empAdd(EmpVO empvo, Model model) {
		log.info("empAdd()실행");
		
		empService.add(empvo);
		empService.addDept(empvo);
	
		return "redirect:list";
		
	}
}
```
```java
//Sercive
package edu.bit.ex.service;

import java.util.List;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

public interface EmpService {
	
	public List<EmpVO> empList();

	public List<EmpVO> mgrList();

	public List<EmpVO> dnameList();

	public void add(EmpVO empvo);

	public void addDept(EmpVO empvo);

	public List<EmpVO> empPage(Criteria cri); //paging 

	public int getTotal(Criteria cri); // 총 page처리
}
```
```java
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
	public List<EmpVO> mgrList() {
		
		return empMapper.empMgrList();
	}

	@Override
	public List<EmpVO> dnameList() {
		
		return empMapper.empDnameList();
	}

	@Override
	public void add(EmpVO empvo) {
		empMapper.empInsert(empvo);
		
	}

	@Override
	public void addDept(EmpVO empvo) {
		empMapper.empDeptInsert(empvo);
		
	}

	@Override
	public List<EmpVO> empPage(Criteria cri) {
		
		return empMapper.empListPage(cri);
	}

	@Override
	public int getTotal(Criteria cri) {
		
		return empMapper.getTotal(cri);
	}
}
```
```java
//Mapper
package edu.bit.ex.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import edu.bit.ex.page.Criteria;
import edu.bit.ex.vo.EmpVO;

@Mapper
public interface EmpMapper {

	public List<EmpVO> empList();

	public List<EmpVO> empMgrList();

	public List<EmpVO> empDnameList();

	public void empInsert(EmpVO empvo);

	public void empDeptInsert(EmpVO empvo);

	public List<EmpVO> empListPage(Criteria cri);

	public int getTotal(Criteria cri);
}
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="edu.bit.ex.mapper.EmpMapper">


<select id="empList" resultType="edu.bit.ex.vo.EmpVO"> 
<![CDATA[ 
select empno, ename, job, mgr, hiredate, sal, comm, deptno from emp
]]>
</select>

<select id="empMgrList" resultType="edu.bit.ex.vo.EmpVO"> 
<![CDATA[ 
select e1.empno, e1.ename from emp e1, emp e2 where e1.empno = e2.mgr group by e1.empno, e1.ename
]]>
</select>

<select id="empDnameList" resultType="edu.bit.ex.vo.EmpVO"> 
<![CDATA[ 
select e.deptno, d.dname from emp e, dept d where  e.deptno = d.deptno group by e.deptno, d.dname
]]>
</select>

<insert id="empInsert">
<![CDATA[
insert into emp (empno, ename, job, mgr, hiredate, sal, comm, deptno) values (#{empno}, #{ename}, #{job}, #{mgr}, #{hiredate}, #{sal}, #{comm}, #{deptno})  

]]>
</insert>

<insert id="empDeptInsert">
<![CDATA[
insert into dept (deptno, dname) values (#{deptno}, #{dname})  

]]>
</insert>

<select id="empListPage" resultType="edu.bit.ex.vo.EmpVO"> 
<![CDATA[ 
select * from 
(select rownum , a.* 
from (select * from emp) a where rownum <= #{page} * #{amount}) where rownum > (#{page} -1 * #{amount})
]]>
</select>

<select id="getTotal" resultType="int"> 
<![CDATA[ 
select count(*) from emp
]]>
</select>

</mapper>
```
```jsp
//list(부트스트랩적용)
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ page session="false"%>
<html>
<head>

<title>Emp List</title>


<!-- Custom fonts for this template -->
<link href="vendor/fontawesome-free/css/all.min.css" rel="stylesheet"
	type="text/css">
<link
	href="https://fonts.googleapis.com/css?family=Nunito:200,200i,300,300i,400,400i,600,600i,700,700i,800,800i,900,900i"
	rel="stylesheet">

<!-- Custom styles for this template -->
<link href="/ex/resources/sb_admin/css/sb-admin-2.min.css"
	rel="stylesheet">

<!-- Custom styles for this page -->
<link href="vendor/datatables/dataTables.bootstrap4.min.css"
	rel="stylesheet">
	
	<style>
		.addCenter {
			text-align: center;
		}
	</style>

</head>

<body id="page-top">
	<!-- Page Wrapper -->
	<div id="wrapper">

		<!-- Sidebar -->
		<ul
			class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion"
			id="accordionSidebar">

			<!-- Sidebar - Brand -->
			<a
				class="sidebar-brand d-flex align-items-center justify-content-center"
				href="index.html">
				<div class="sidebar-brand-icon rotate-n-15">
					<i class="fas fa-laugh-wink"></i>
				</div>
				<div class="sidebar-brand-text mx-3">
					SB Admin <sup>2</sup>
				</div>
			</a>

			<!-- Divider -->
			<hr class="sidebar-divider my-0">

			<!-- Nav Item - Dashboard -->
			<li class="nav-item"><a class="nav-link" href="index.html">
					<i class="fas fa-fw fa-tachometer-alt"></i> <span>Dashboard</span>
			</a></li>

			<!-- Divider -->
			<hr class="sidebar-divider">

			<!-- Heading -->
			<div class="sidebar-heading">Interface</div>

			<!-- Nav Item - Pages Collapse Menu -->
			<li class="nav-item"><a class="nav-link collapsed" href="#"
				data-toggle="collapse" data-target="#collapseTwo"
				aria-expanded="true" aria-controls="collapseTwo"> <i
					class="fas fa-fw fa-cog"></i> <span>Components</span>
			</a>
				<div id="collapseTwo" class="collapse" aria-labelledby="headingTwo"
					data-parent="#accordionSidebar">
					<div class="bg-white py-2 collapse-inner rounded">
						<h6 class="collapse-header">Custom Components:</h6>
						<a class="collapse-item" href="buttons.html">Buttons</a> <a
							class="collapse-item" href="cards.html">Cards</a>
					</div>
				</div></li>

			<!-- Nav Item - Utilities Collapse Menu -->
			<li class="nav-item"><a class="nav-link collapsed" href="#"
				data-toggle="collapse" data-target="#collapseUtilities"
				aria-expanded="true" aria-controls="collapseUtilities"> <i
					class="fas fa-fw fa-wrench"></i> <span>Utilities</span>
			</a>
				<div id="collapseUtilities" class="collapse"
					aria-labelledby="headingUtilities" data-parent="#accordionSidebar">
					<div class="bg-white py-2 collapse-inner rounded">
						<h6 class="collapse-header">Custom Utilities:</h6>
						<a class="collapse-item" href="utilities-color.html">Colors</a> <a
							class="collapse-item" href="utilities-border.html">Borders</a> <a
							class="collapse-item" href="utilities-animation.html">Animations</a>
						<a class="collapse-item" href="utilities-other.html">Other</a>
					</div>
				</div></li>

			<!-- Divider -->
			<hr class="sidebar-divider">

			<!-- Heading -->
			<div class="sidebar-heading">Addons</div>

			<!-- Nav Item - Pages Collapse Menu -->
			<li class="nav-item"><a class="nav-link collapsed" href="#"
				data-toggle="collapse" data-target="#collapsePages"
				aria-expanded="true" aria-controls="collapsePages"> <i
					class="fas fa-fw fa-folder"></i> <span>Pages</span>
			</a>
				<div id="collapsePages" class="collapse"
					aria-labelledby="headingPages" data-parent="#accordionSidebar">
					<div class="bg-white py-2 collapse-inner rounded">
						<h6 class="collapse-header">Login Screens:</h6>
						<a class="collapse-item" href="login.html">Login</a> <a
							class="collapse-item" href="register.html">Register</a> <a
							class="collapse-item" href="forgot-password.html">Forgot
							Password</a>
						<div class="collapse-divider"></div>
						<h6 class="collapse-header">Other Pages:</h6>
						<a class="collapse-item" href="404.html">404 Page</a> <a
							class="collapse-item" href="blank.html">Blank Page</a>
					</div>
				</div></li>

			<!-- Nav Item - Charts -->
			<li class="nav-item"><a class="nav-link" href="charts.html">
					<i class="fas fa-fw fa-chart-area"></i> <span>Charts</span>
			</a></li>

			<!-- Nav Item - Tables -->
			<li class="nav-item active"><a class="nav-link"
				href="tables.html"> <i class="fas fa-fw fa-table"></i> <span>Tables</span></a>
			</li>

			<!-- Divider -->
			<hr class="sidebar-divider d-none d-md-block">

			<!-- Sidebar Toggler (Sidebar) -->
			<div class="text-center d-none d-md-inline">
				<button class="rounded-circle border-0" id="sidebarToggle"></button>
			</div>

		</ul>
		<!-- End of Sidebar -->

		<!-- Content Wrapper -->
		<div id="content-wrapper" class="d-flex flex-column">

			<!-- Main Content -->
			<div id="content">

				<!-- Topbar -->
				<nav
					class="navbar navbar-expand navbar-light bg-white topbar mb-4 static-top shadow">

					<!-- Sidebar Toggle (Topbar) -->
					<button id="sidebarToggleTop"
						class="btn btn-link d-md-none rounded-circle mr-3">
						<i class="fa fa-bars"></i>
					</button>

					<!-- Topbar Search -->
					<form
						class="d-none d-sm-inline-block form-inline mr-auto ml-md-3 my-2 my-md-0 mw-100 navbar-search">
						<div class="input-group">
							<input type="text" class="form-control bg-light border-0 small"
								placeholder="Search for..." aria-label="Search"
								aria-describedby="basic-addon2">
							<div class="input-group-append">
								<button class="btn btn-primary" type="button">
									<i class="fas fa-search fa-sm"></i>
								</button>
							</div>
						</div>
					</form>

					<!-- Topbar Navbar -->
					<ul class="navbar-nav ml-auto">

						<!-- Nav Item - Search Dropdown (Visible Only XS) -->
						<li class="nav-item dropdown no-arrow d-sm-none"><a
							class="nav-link dropdown-toggle" href="#" id="searchDropdown"
							role="button" data-toggle="dropdown" aria-haspopup="true"
							aria-expanded="false"> <i class="fas fa-search fa-fw"></i>
						</a> <!-- Dropdown - Messages -->
							<div
								class="dropdown-menu dropdown-menu-right p-3 shadow animated--grow-in"
								aria-labelledby="searchDropdown">
								<form class="form-inline mr-auto w-100 navbar-search">
									<div class="input-group">
										<input type="text"
											class="form-control bg-light border-0 small"
											placeholder="Search for..." aria-label="Search"
											aria-describedby="basic-addon2">
										<div class="input-group-append">
											<button class="btn btn-primary" type="button">
												<i class="fas fa-search fa-sm"></i>
											</button>
										</div>
									</div>
								</form>
							</div></li>

						<!-- Nav Item - Alerts -->
						<li class="nav-item dropdown no-arrow mx-1"><a
							class="nav-link dropdown-toggle" href="#" id="alertsDropdown"
							role="button" data-toggle="dropdown" aria-haspopup="true"
							aria-expanded="false"> <i class="fas fa-bell fa-fw"></i> <!-- Counter - Alerts -->
								<span class="badge badge-danger badge-counter">3+</span>
						</a> <!-- Dropdown - Alerts -->
							<div
								class="dropdown-list dropdown-menu dropdown-menu-right shadow animated--grow-in"
								aria-labelledby="alertsDropdown">
								<h6 class="dropdown-header">Alerts Center</h6>
								<a class="dropdown-item d-flex align-items-center" href="#">
									<div class="mr-3">
										<div class="icon-circle bg-primary">
											<i class="fas fa-file-alt text-white"></i>
										</div>
									</div>
									<div>
										<div class="small text-gray-500">December 12, 2019</div>
										<span class="font-weight-bold">A new monthly report is
											ready to download!</span>
									</div>
								</a> <a class="dropdown-item d-flex align-items-center" href="#">
									<div class="mr-3">
										<div class="icon-circle bg-success">
											<i class="fas fa-donate text-white"></i>
										</div>
									</div>
									<div>
										<div class="small text-gray-500">December 7, 2019</div>
										$290.29 has been deposited into your account!
									</div>
								</a> <a class="dropdown-item d-flex align-items-center" href="#">
									<div class="mr-3">
										<div class="icon-circle bg-warning">
											<i class="fas fa-exclamation-triangle text-white"></i>
										</div>
									</div>
									<div>
										<div class="small text-gray-500">December 2, 2019</div>
										Spending Alert: We've noticed unusually high spending for your
										account.
									</div>
								</a> <a class="dropdown-item text-center small text-gray-500"
									href="#">Show All Alerts</a>
							</div></li>

						<!-- Nav Item - Messages -->
						<li class="nav-item dropdown no-arrow mx-1"><a
							class="nav-link dropdown-toggle" href="#" id="messagesDropdown"
							role="button" data-toggle="dropdown" aria-haspopup="true"
							aria-expanded="false"> <i class="fas fa-envelope fa-fw"></i>
								<!-- Counter - Messages --> <span
								class="badge badge-danger badge-counter">7</span>
						</a> <!-- Dropdown - Messages -->
							<div
								class="dropdown-list dropdown-menu dropdown-menu-right shadow animated--grow-in"
								aria-labelledby="messagesDropdown">
								<h6 class="dropdown-header">Message Center</h6>
								<a class="dropdown-item d-flex align-items-center" href="#">
									<div class="dropdown-list-image mr-3">
										<img class="rounded-circle"
											src="https://source.unsplash.com/fn_BT9fwg_E/60x60" alt="">
										<div class="status-indicator bg-success"></div>
									</div>
									<div class="font-weight-bold">
										<div class="text-truncate">Hi there! I am wondering if
											you can help me with a problem I've been having.</div>
										<div class="small text-gray-500">Emily Fowler · 58m</div>
									</div>
								</a> <a class="dropdown-item d-flex align-items-center" href="#">
									<div class="dropdown-list-image mr-3">
										<img class="rounded-circle"
											src="https://source.unsplash.com/AU4VPcFN4LE/60x60" alt="">
										<div class="status-indicator"></div>
									</div>
									<div>
										<div class="text-truncate">I have the photos that you
											ordered last month, how would you like them sent to you?</div>
										<div class="small text-gray-500">Jae Chun · 1d</div>
									</div>
								</a> <a class="dropdown-item d-flex align-items-center" href="#">
									<div class="dropdown-list-image mr-3">
										<img class="rounded-circle"
											src="https://source.unsplash.com/CS2uCrpNzJY/60x60" alt="">
										<div class="status-indicator bg-warning"></div>
									</div>
									<div>
										<div class="text-truncate">Last month's report looks
											great, I am very happy with the progress so far, keep up the
											good work!</div>
										<div class="small text-gray-500">Morgan Alvarez · 2d</div>
									</div>
								</a> <a class="dropdown-item d-flex align-items-center" href="#">
									<div class="dropdown-list-image mr-3">
										<img class="rounded-circle"
											src="https://source.unsplash.com/Mv9hjnEUHR4/60x60" alt="">
										<div class="status-indicator bg-success"></div>
									</div>
									<div>
										<div class="text-truncate">Am I a good boy? The reason I
											ask is because someone told me that people say this to all
											dogs, even if they aren't good...</div>
										<div class="small text-gray-500">Chicken the Dog · 2w</div>
									</div>
								</a> <a class="dropdown-item text-center small text-gray-500"
									href="#">Read More Messages</a>
							</div></li>

						<div class="topbar-divider d-none d-sm-block"></div>

						<!-- Nav Item - User Information -->
						<li class="nav-item dropdown no-arrow"><a
							class="nav-link dropdown-toggle" href="#" id="userDropdown"
							role="button" data-toggle="dropdown" aria-haspopup="true"
							aria-expanded="false"> <span
								class="mr-2 d-none d-lg-inline text-gray-600 small">Valerie
									Luna</span> <img class="img-profile rounded-circle"
								src="https://source.unsplash.com/QAB-WJcbgJk/60x60">
						</a> <!-- Dropdown - User Information -->
							<div
								class="dropdown-menu dropdown-menu-right shadow animated--grow-in"
								aria-labelledby="userDropdown">
								<a class="dropdown-item" href="#"> <i
									class="fas fa-user fa-sm fa-fw mr-2 text-gray-400"></i> Profile
								</a> <a class="dropdown-item" href="#"> <i
									class="fas fa-cogs fa-sm fa-fw mr-2 text-gray-400"></i>
									Settings
								</a> <a class="dropdown-item" href="#"> <i
									class="fas fa-list fa-sm fa-fw mr-2 text-gray-400"></i>
									Activity Log
								</a>
								<div class="dropdown-divider"></div>
								<a class="dropdown-item" href="#" data-toggle="modal"
									data-target="#logoutModal"> <i
									class="fas fa-sign-out-alt fa-sm fa-fw mr-2 text-gray-400"></i>
									Logout
								</a>
							</div></li>

					</ul>

				</nav>
				<!-- End of Topbar -->


				<!-- Begin Page Content -->
				<div class="container-fluid">

					<!-- Page Heading -->
					<h1 class="h3 mb-2 text-gray-800">Tables</h1>
					<p class="mb-4">
						DataTables is a third party plugin that is used to generate the
						demo table below. For more information about DataTables, please
						visit the <a target="_blank" href="https://datatables.net">official
							DataTables documentation</a>.
					</p>

					<!-- DataTales Example -->
					<div class="card shadow mb-4">
						<div class="card-header py-3">
							<h6 class="m-0 font-weight-bold text-primary">EMP List</h6>
						</div>
						<div class="card-body">
							<div class="table-responsive">
								<table class="table table-bordered" id="dataTable" width="100%"
									cellspacing="0">
									<tr>
										<td>사원번호</td>
										<td>사원이름</td>
										<td>사원직급</td>
										<td>매니저</td>
										<td>입사일</td>
										<td>급여</td>
										<td>커미션</td>
										<td>부서번호</td>
									</tr>
									<c:forEach items="${empList}" var="dao">
										<tr>
											<td>${dao.empno}</td>
											<td>${dao.ename}</td>
											<td>${dao.job}</td>
											<td>${dao.mgr}</td>
											<td>${dao.hiredate}</td>
											<td>${dao.sal}</td>
											<td>${dao.comm}</td>
											<td>${dao.deptno}</td>
										</tr>
									</c:forEach>
									
									<tr class="addCenter">
										<td colspan="8" style=""><a href="EmpAdd_View">사원 추가</a></td>									
									</tr>
								</table>
								
                						<!-- 이전이나 다음이 나오지 않는 이유 10단위로 움직이기 때문에 당연히 그것보다 갯수가 적으면 안나옴 -->
								<c:if test="${pageMaker.previus}">
									<a href = "empPage${pageMaker.makeQuery(pageMaker.startPage - 1)}"> 이전 </a>
								</c:if>
								
								<c:forEach begin="${pageMaker.startPage}" end="${pageMaker.endPage}" var="idx">
									<c:out value="${pageMaker.cri.page == idx?'':''}" /> 
								<!-- 삼항연산자, idx 혹시나 값이 잘못넘어오면 아무것도 넣지 않게 하기위한 하나의 에러처리문 여기서는 필요없음 -->
									
									<a href = "empPage${pageMaker.makeQuery(idx)}">${idx}</a>
									<!-- empPage?pageNum=2&amount=10 -->
									<!-- <a href = "empPage?pageNum=${idx}&amount=10">${idx}</a> -->
								</c:forEach>
								
								<c:if test= "${pageMaker.next && pageMaker.endPage > 0}">
									<a href ="empPage${pageMaker.makeQuery(pageMaker.endPage +1)}"> 다음 </a>
								</c:if>
								<br>
							</div>
						</div>
					</div>

				</div>
				<!-- /.container-fluid -->

			</div>
			<!-- End of Main Content -->

			<!-- Footer -->
			<footer class="sticky-footer bg-white">
				<div class="container my-auto">
					<div class="copyright text-center my-auto">
						<span>Copyright &copy; Your Website 2019</span>
					</div>
				</div>
			</footer>
			<!-- End of Footer -->

		</div>
		<!-- End of Content Wrapper -->

	</div>
	<!-- End of Page Wrapper -->

	<!-- Scroll to Top Button-->
	<a class="scroll-to-top rounded" href="#page-top"> <i
		class="fas fa-angle-up"></i>
	</a>

	<!-- Logout Modal-->
	<div class="modal fade" id="logoutModal" tabindex="-1" role="dialog"
		aria-labelledby="exampleModalLabel" aria-hidden="true">
		<div class="modal-dialog" role="document">
			<div class="modal-content">
				<div class="modal-header">
					<h5 class="modal-title" id="exampleModalLabel">Ready to Leave?</h5>
					<button class="close" type="button" data-dismiss="modal"
						aria-label="Close">
						<span aria-hidden="true">×</span>
					</button>
				</div>
				<div class="modal-body">Select "Logout" below if you are ready
					to end your current session.</div>
				<div class="modal-footer">
					<button class="btn btn-secondary" type="button"
						data-dismiss="modal">Cancel</button>
					<a class="btn btn-primary" href="login.html">Logout</a>
				</div>
			</div>
		</div>
	</div>

	<!-- Bootstrap core JavaScript-->
	<script src="vendor/jquery/jquery.min.js"></script>
	<script src="vendor/bootstrap/js/bootstrap.bundle.min.js"></script>

	<!-- Core plugin JavaScript-->
	<script src="vendor/jquery-easing/jquery.easing.min.js"></script>

	<!-- Custom scripts for all pages-->
	<script src="js/sb-admin-2.min.js"></script>

	<!-- Page level plugins -->
	<script src="vendor/datatables/jquery.dataTables.min.js"></script>
	<script src="vendor/datatables/dataTables.bootstrap4.min.js"></script>

	<!-- Page level custom scripts -->
	<script src="js/demo/datatables-demo.js"></script>

</body>
</html>
```

# 2. 오라클 11g 이하에서의 페이징 처리 방법(DB)은?
- rownum을 활용해 pasing 처리 
- select가 3중으로 처리되서 '3중세트'라고도 함
```sql
select rownum rn, a.* from emp a; 
-- a.라고 표시를 해주지않으면 어디에서 all인지 모르기 때문에 에러가난다 (어디에서 컬럼을 가져올지 꼭 명시해줘야함)
```
<br>

# 3. rownum 에 대하여 설명하시오
- 해당 컬럼들에 대한 **행번호를 부여**해주는 요소 
- 오라클 전용 (mySql같은 경우는 limit를 사용해 페이징처리를 한다)

## @ (*)rownum의 처리 순서
```sql
[sql 처리 순서]

1. from , where절 처리 

>> rownum처리 (select 처리되기 전 where절의 조건까지 확인 후 번호를 할당한다) 

2. select 처리 
3. group by 조건 적용
4. having 적용
5. order by 적용
```

