# 1. 아래와 같이 나오게 프로그래밍하시오
- http://locallhost:8282/context명/list.do 을 치면 나오게 하면됨 
![주석 2021-01-16 175608](https://user-images.githubusercontent.com/74290204/104807707-323f2c80-5824-11eb-8e1c-52a6f7eac98f.png)

### [알아야 접근가능한것!] 외부 소스코드를 어떻게 적용할것인가? link 이용하는 방법
- link는 현재 페이지 말고 다른 외부페이지의 css속성을 가져와 적용하는것, 외부 리소스를 받아오는 태그

```
[힌트] 짜고 있는 게시판 소스코드에 sb admin 폴더에서 css 및 img 등등을
webcontent 아래로 복사하고 table.jsp를 분석한 후 위와 같이 나오도록 프로그래밍!
```

```jsp
// Answer 
//list_viwe.jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>


  <!-- Custom fonts for this template -->
  <link href="vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">
  <link href="https://fonts.googleapis.com/css?family=Nunito:200,200i,300,300i,400,400i,600,600i,700,700i,800,800i,900,900i" rel="stylesheet">

  <!-- Custom styles for this template -->
  <link href="css/sb-admin-2.min.css" rel="stylesheet">

  <!-- Custom styles for this page -->
  <link href="vendor/datatables/dataTables.bootstrap4.min.css" rel="stylesheet">

</head>
<body id="page-top">

<div id="wrapper">	
 <!-- Sidebar -->
    <ul class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion" id="accordionSidebar">

      <!-- Sidebar - Brand -->
      <a class="sidebar-brand d-flex align-items-center justify-content-center" href="index.html">
        <div class="sidebar-brand-icon rotate-n-15">
          <i class="fas fa-laugh-wink"></i>
        </div>
        <div class="sidebar-brand-text mx-3">SB Admin <sup>2</sup></div>
      </a>

      <!-- Divider -->
      <hr class="sidebar-divider my-0">

      <!-- Nav Item - Dashboard -->
      <li class="nav-item">
        <a class="nav-link" href="index.html">
          <i class="fas fa-fw fa-tachometer-alt"></i>
          <span>Dashboard</span></a>
      </li>

      <!-- Divider -->
      <hr class="sidebar-divider">

      <!-- Heading -->
      <div class="sidebar-heading">
        Interface
      </div>

      <!-- Nav Item - Pages Collapse Menu -->
      <li class="nav-item">
        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="true" aria-controls="collapseTwo">
          <i class="fas fa-fw fa-cog"></i>
          <span>Components</span>
        </a>
        <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordionSidebar">
          <div class="bg-white py-2 collapse-inner rounded">
            <h6 class="collapse-header">Custom Components:</h6>
            <a class="collapse-item" href="buttons.html">Buttons</a>
            <a class="collapse-item" href="cards.html">Cards</a>
          </div>
        </div>
      </li>

      <!-- Nav Item - Utilities Collapse Menu -->
      <li class="nav-item">
        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapseUtilities" aria-expanded="true" aria-controls="collapseUtilities">
          <i class="fas fa-fw fa-wrench"></i>
          <span>Utilities</span>
        </a>
        <div id="collapseUtilities" class="collapse" aria-labelledby="headingUtilities" data-parent="#accordionSidebar">
          <div class="bg-white py-2 collapse-inner rounded">
            <h6 class="collapse-header">Custom Utilities:</h6>
            <a class="collapse-item" href="utilities-color.html">Colors</a>
            <a class="collapse-item" href="utilities-border.html">Borders</a>
            <a class="collapse-item" href="utilities-animation.html">Animations</a>
            <a class="collapse-item" href="utilities-other.html">Other</a>
          </div>
        </div>
      </li>

      <!-- Divider -->
      <hr class="sidebar-divider">

      <!-- Heading -->
      <div class="sidebar-heading">
        Addons
      </div>

      <!-- Nav Item - Pages Collapse Menu -->
      <li class="nav-item">
        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapsePages" aria-expanded="true" aria-controls="collapsePages">
          <i class="fas fa-fw fa-folder"></i>
          <span>Pages</span>
        </a>
        <div id="collapsePages" class="collapse" aria-labelledby="headingPages" data-parent="#accordionSidebar">
          <div class="bg-white py-2 collapse-inner rounded">
            <h6 class="collapse-header">Login Screens:</h6>
            <a class="collapse-item" href="login.html">Login</a>
            <a class="collapse-item" href="register.html">Register</a>
            <a class="collapse-item" href="forgot-password.html">Forgot Password</a>
            <div class="collapse-divider"></div>
            <h6 class="collapse-header">Other Pages:</h6>
            <a class="collapse-item" href="404.html">404 Page</a>
            <a class="collapse-item" href="blank.html">Blank Page</a>
          </div>
        </div>
      </li>

      <!-- Nav Item - Charts -->
      <li class="nav-item">
        <a class="nav-link" href="charts.html">
          <i class="fas fa-fw fa-chart-area"></i>
          <span>Charts</span></a>
      </li>

      <!-- Nav Item - Tables -->
      <li class="nav-item active">
        <a class="nav-link" href="tables.html">
          <i class="fas fa-fw fa-table"></i>
          <span>Tables</span></a>
      </li>

      <!-- Divider -->
      <hr class="sidebar-divider d-none d-md-block">

      <!-- Sidebar Toggler (Sidebar) -->
      <div class="text-center d-none d-md-inline">
        <button class="rounded-circle border-0" id="sidebarToggle"></button>
      </div>

    </ul> <!-- End of Sidebar -->
	
   <div class="container-fluid"> <!-- Begin Page Content -->
   
   		<!-- Page Heading -->
		<h1 class="h3 mb-2 text-gray-800">Tables</h1>
		<p class="mb-4">DataTables is a third party plugin that is used to generate the demo table below. For more information about DataTables, please visit the <a target="_blank" href="https://datatables.net">official DataTables documentation</a>.</p>

          <!-- DataTales Example -->
		<div class="card shadow mb-4">
			<div class="card-header py-3">
				<h6 class="m-0 font-weight-bold text-primary">DataTables Example</h6>
			</div>
			
			<div style="padding: 20px;"> <!--  div로 줘도 안먹어서 직접 지정해주는데 왜지? -->
			<form>
				Show <select name="entry_view">
					<c:forEach begin="1" end="10" var="entry">
					<option>${entry}</option>
					</c:forEach>
				</select> entries &nbsp; &nbsp; 
				
				<span style="float: right">Search: <input type="text" name="search" size="20"></span>
			</form>
			</div>
	        <div class="card-body">
	        	<div class="table-responsive">
					<table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
						<tr>
							<th>번호</th> <!-- 기본적으로 th는 head를 의미하고 th로 주면 자동으로 볼드처리가된다 -->
							<th>이름</th>
							<th>제목</th>
							<th>날짜</th>
							<th>히트</th>
						</tr>
	
							<c:forEach items="${list}" var="dto" > <!-- 달라표시 태그..ㅎㅎ.. -->
							<tr>
							<td>${dto.bId}</td>
							<td>${dto.bName}</td>
							<td>
							<c:forEach begin="1" end="${dto.bIndent}">[re]</c:forEach>
							<a href="content_view.do?bId=${dto.bId}">${dto.bTitle}</a></td>
							<td>${dto.bDate}</td>
							<td>${dto.bHit}</td>
						</tr>
							</c:forEach>
					
					</table>
					<div>
					<span>Showing 1 to 4 of 4 entries </span>
					
					<span style="float: right" >
						<form>
							<input type="button" value="Previous">
							<select>
							<option type="selected">1</option>
							</select>
							<input type="button" value="Next">
					
						</form>
					</span>
					</div>
					<div class="copyright text-center my-auto">
           				 <span><a href = "writ_view.do">글 작성</a></span>
         			</div>
					
					
					
				</div>
			</div>
		</div>
	</div> <!-- /.container-fluid -->
</div>

</body>
</html>
```
## ▶ 출력 결과

![주석 2021-01-16 175526](https://user-images.githubusercontent.com/74290204/104807780-daed8c00-5824-11eb-8aa5-ac7b5268d0db.png)
