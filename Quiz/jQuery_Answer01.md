# 1. 아래를 프로그래밍하시오
![q1](https://user-images.githubusercontent.com/74290204/106134356-c2c52780-61a9-11eb-848b-6f23ce750a51.PNG)

## - 페이징 처리하는 부분이 .말고 변경을 어떻게 하는지 모르겠다(링크가 아마 받아오는것같은데..흠..)
```jsp
<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<title></title>
<script src="http://code.jquery.com/jquery-latest.min.js"></script>
</head>
<body>

<!-- 이 예제에서는 필요한 js, css 를 링크걸어 사용 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.5.1/css/swiper.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.5.1/js/swiper.min.js"></script>

<style type="text/css">

.swiper-container {
	width:650px;
	height:440px;
	border:5px solid silver;
	border-radius:5px;
	box-shadow:0 0 20px #ccc inset;
}
.swiper-slide {
	text-align:center;
	display:flex; /* 내용을 중앙정렬 하기위해 flex 사용 */
	align-items:center; /* 위아래 기준 중앙정렬 */
	justify-content:center; /* 좌우 기준 중앙정렬 */
}
.swiper-slide img {
	border:1px solid #000;
	box-shadow:7px 7px 2px #ccc;
}

</style>

<!-- 클래스명은 변경하면 안 됨 -->
<div class="swiper-container">
	<div class="swiper-wrapper">
		<div class="swiper-slide"><img data-src="https://t1.daumcdn.net/cfile/tistory/242EF23458ECE44A2B" class="swiper-lazy"></div>
		<div class="swiper-slide"><img data-src="https://i.pinimg.com/736x/5f/96/49/5f96498927c8b592c18dd87939af2d97.jpg" class="swiper-lazy"></div>
		<div class="swiper-slide"><img data-src="https://resources.matcha-jp.com/old_thumbnails/720x2000/1332.jpg" class="swiper-lazy"></div>
		<div class="swiper-slide"><img data-src="https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F1450B43350C7E9F83841FE" class="swiper-lazy"></div>
	
	</div>
	
	<!-- 네비게이션 버튼 -->
	<div class="swiper-button-next"></div><!-- 다음 버튼 (오른쪽에 있는 버튼) -->
	<div class="swiper-button-prev"></div><!-- 이전 버튼 -->

	
</div>
<!-- 페이징 -->
	<div class="swiper-pagination">
		<img data-src="https://t1.daumcdn.net/cfile/tistory/242EF23458ECE44A2B">
		<img data-src="https://i.pinimg.com/736x/5f/96/49/5f96498927c8b592c18dd87939af2d97.jpg">
		<img data-src="https://resources.matcha-jp.com/old_thumbnails/720x2000/1332.jpg">
		<img data-src="https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F1450B43350C7E9F83841FE">
	</div>
		
</div>
<script>

new Swiper('.swiper-container', {

	// ★동적로딩 설정
	lazy : {
		loadPrevNext : true // 이전, 다음 이미지는 미리 로딩
	},

	// 페이징 설정
	pagination : {
		el : '.swiper-pagination',
		clickable : true, // 페이징을 클릭하면 해당 영역으로 이동, 필요시 지정해 줘야 기능 작동
	},

	// 네비게이션 설정
	navigation : {
		nextEl : '.swiper-button-next', // 다음 버튼 클래스명
		prevEl : '.swiper-button-prev', // 이번 버튼 클래스명
	},
});

</script>

</body>
</html>
```

# 2. 아래를 프로그래밍하시오

![q2](https://user-images.githubusercontent.com/74290204/106134305-b3de7500-61a9-11eb-9220-00ab1c90fffe.PNG)


```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Click</title>
<link rel="stylesheet"
	href="//code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
<link rel="stylesheet" href="/resources/demos/style.css">

<script
	src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>

<script src="https://code.jquery.com/jquery-1.12.4.js"></script>
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>
<script>
  $( function() {
    $( "#datepicker" ).datepicker();
  } );
	
  </script>
</head>

<body>

	<p>
		Date: <input type="text" id="datepicker">
	</p>

</body>
</html>
```
