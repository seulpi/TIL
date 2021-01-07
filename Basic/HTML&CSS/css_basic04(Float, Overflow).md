# - float : position과 마찬가지로 위치를 설정하기 위한 속성
>> float : 띄운다의 의미
```html
<style>
	float : left
</style>

▶ 프로그래밍 내부적으로는 왼쪽으로가서 공중으로 띄운다는 뜻 
```
- [float 원리] float을 하면 다음에 올 영역이 바로 밑으로 들어가게되서 display되지 않음(띄워져있어서 가리니까)
- [에외!] float을 하면 당연히 영역이 float한 영역 밑으로 쌓여야하는데 **글자는 예외!** <br>
why? css의 목적은 사용을 쉽게하기 위함이기 때문에 기능적으로 따로 추가된 부분 
```
원리는 글자의 컨텐의 면적은 float 영역 밑에서부터 시작되지만 display는 float영역 밖에 보이게 된다
```

# - overflow : 부모보다 큰 자식의 영역을 컨트롤하는 속성
- 부모의 크키(width,height)는 따로 지정해주지 않으면 자식에게 영향을 받아 자식의 크기에 맞게 자동으로 크기가 설정된다 <br>
☞ overflow는 부모의 크기와 자식의 크기를 각자 줬을 경우 자식의 크기가 부모보다 작은 경우에 사용된다

## @ 종류
1. visible : 기본값! , 영역이 넘칠 경우 상자 밖으로 영역 보여주기
>> overflow: visible;
2. hidden : 영역이 넘치는 부분 가려주기(안보임)
>> overflow: hidden;
3. scroll : 영역이 넘치면 스크롤 바 추가되서 스크롤 (가로, 세로 모두 추가 가능)
>> overflow: scroll;
4. auto : 컨텐츠 양에 따라 스크롤바를 추가할 지 자동으로 결정(필요에 따라 가로, 세로 별도로 지정 가능)
>>overflow: auto;

###  ▶ [overflow 설명 영상 참고] https://www.youtube.com/watch?v=O-n1EjDEuIc 

- 블록 서식 맥락(Block Format Context) : 독립적인 공간을 만드는 것 → (자식틀의 영역을 컨트롤 = 자식들에 영향을 받는다)

- 반응형 웹(Responsive Web) : 화면크기에 따라 반응하는 웹 페이지, 모바일의 수요 시장이 커지면서 매우 중요해짐
