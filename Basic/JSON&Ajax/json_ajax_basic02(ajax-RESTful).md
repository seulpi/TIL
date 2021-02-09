# RESTful 
- 실무에서ㅓ의 **RESTful은 URI 설계**
```js
/board/board.jpg → resource 위치 (url, location) 
/board/1000 → board의 1000번째 글 (uri , idenfication)
▶ 1000번째 + Select인지 , Update인지 Delete인지 Insert인지

▶ REST API : /board/1000 
```
- Create Read Udatate Delete (RESEful, 설계) 
- Spring을 위한 개념이 아니라 하나의 통합적인 개념 
- 주소 **경로**는 왠만하면 **다 소문자로~**

![Rest API](https://user-images.githubusercontent.com/74290204/107378347-3d3d6200-6b2f-11eb-99e4-5ca2554ecfc1.png)

```java
//RestBoardController.java
package edu.bit.ex.board.controller;

import javax.servlet.jsp.PageContext;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

import edu.bit.ex.board.service.BService;
import edu.bit.ex.board.service.BServiceImpl;
import edu.bit.ex.board.vo.BoardVO;
import lombok.AllArgsConstructor;
import lombok.extern.log4j.Log4j;



//REST : Representational State Transfer
//하나의 URI가 하나의 고유한 리소스를 대표하도록 설계된 개념

//http://localhost/spring02/list?bno=1 ==> url+파라미터
//http://localhost/spring02/list/1 ==> url
//RestController은 스프링 4.0부터 지원
//@Controller, @RestController 차이점은 메서드가 종료되면 화면전환의 유무

@Log4j
@AllArgsConstructor
@RestController
@RequestMapping("/restful/*")
public class RestBoardController {

	private BServiceImpl bSerivce;
	
	//기존 @Controller 방식으로 넘기려면 ModelAndView를 사용해 데이터와 view를 넘겨줘야함
	// 1. list(처음 진입 화면이므로 화면이 깜박여도 상관없기 때문에 @Controller방식으로 접근 -> 꼭 json으로 넘겨야 된다 그런거아님
	@GetMapping("/board")
	public ModelAndView list(ModelAndView mav) {
		mav.setViewName("rest_list");
		mav.addObject("list", bSerivce.getList());
		
		return mav;
		
	}
	
	@GetMapping("/board/{bId}") 
	//bId 처리 방식 -> 1. @PathVariable로 처리 @PathVariable("bId") String bId, Model model 2. Command객체로 처리
	public String rest_content_view(BoardVO boardVO, Model model) {
		
		log.info("rest_content_view");
		model.addAttribute("content_view", bSerivce.getBoard(boardVO.getbId()));
		
		return "content_view";
	}

	@DeleteMapping("/board/{bId}") 
	//delete는 기본적으로 form 태그에서 지원하지 x(get, post만 지원) -> list에서 클릭을 하게되면 js&jquery-ajax로 처리 (submit이런거말고)
	// 어떨 때 사용하느냐? 댓글 삭제 -> ajax
	public ResponseEntity<String> rest_delete(BoardVO boardVO, Model model) {
		
		ResponseEntity<String> entity = null;
		
		log.info("rest_delet");
		
		try {
			bSerivce.remove(boardVO.getbId());
			//삭제가 성공하면 성공 상태 메세지 저장
			entity = new ResponseEntity<String>("SUCCESS", HttpStatus.OK);
											//json (X), 그냥 SUCCESS라는 일반 문자열
		}catch (Exception e) {
			e.printStackTrace();
			//삭제가 실패하면 실패 상태 메세지 저장
			entity = new ResponseEntity<String>(e.getMessage(), HttpStatus.BAD_REQUEST);
		}
		//삭제처리 http 상태 메세지 리턴
		return entity;
	}
}
```
