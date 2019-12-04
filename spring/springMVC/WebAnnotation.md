## @ModelAttribute
- @RequestMapping 어노테이션이 적용되지 않은 별도 메서드로 모델에 추가될 객체 생성(공통 데이터 생성)
  * @ModelAttribute 어노테이션을 메서드에 적용하면 해당 메서드가 생성한 객체가 뷰에 전달 됨
  * "뷰에 모델(Model) 전달(@ModelAttribute("popularFruit")" 게시물 참조
- 커맨드 객체의 초기화 작업 수행
  * @ModelAttribute 어노테이션이 적용된 메서드가 @RequestMapping 어노테이션이 적용된 메서드 보다 먼저 호출 됨
- @ModelAttribute 어노테이션이 적용된 메서드가 사용 가능한 파라미터
  * HttpServletRequest, Locale, @RequestParam 어노테이션 적용 파라미터, @PathVariable 어노테이션 적용 파라미터 등등

## Reference
[Spring Web Annotations | Baeldung](https://www.baeldung.com/spring-mvc-annotations)<br/>
[랄라라 :: @ModelAttribute 어노테이션](https://unabated.tistory.com/entry/ModelAttribute-어노테이션)
