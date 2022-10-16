## Views
https://docs.devexpress.com/WindowsForms/114636/controls-and-libraries/data-grid/views?v=21.2
- 모든 View는 BaseView라는 추상 클래스에서 파생됨.
- ```GridControl.ViewCollection```에 등록된 View 간 전환 가능

## GridView
https://docs.devexpress.com/WindowsForms/3464/controls-and-libraries/data-grid/views/grid-view?v=21.2
- 자동 컬럼 생성
  - ```AutoPopulateColumns``` 속성이 활성화 상태면 바인딩된 데이터 원본의 데이터 필드에 대해 자동으로 열 생성.
  - 데이터 모델 Property에 ```DisplayAttribute```를 설정해 자동 컬럼 생성 대상에서 제외할 수 있음.

- 주요 Method
  - ```BestFitColumns``` : 모든 컬럼에 BestFit 설정 적용.

### GridOptionsView
- 주요 Property
  - ```ColumnAutoWidth``` : Grid 컬럼이 View 경계를 넘을 수 없도록 열 길이를 자동으로 조절 함. 비활성화시 전체 컬럼 사이즈가 View 경계를 초과할 경우 횡스크롤바 표시.
  - ```ShowVerticalLines```
  - ```ShowHorizontalLines```
  - ```ShowIndicator```
  - ```ShowGroupPanel```

## GridColumn
- 주요 Property
  - ```FieldName``` : 바인딩할 데이터의 Field(또는 Property)의 이름. 고유해야 함.
  - ```Caption``` : 컬럼 머리글 Text.
  - ```VisibleWidth``` : 실제 열 너비
  
- 주요 Method
  - ```BestFit``` : 해당 열의 최적 Width 적용.

### OptionsColumn
- 주요 Property
  - ```FixedWitdth``` : ColumnAutoWidth 모드에서 열 크기가 자동으로 조정되지 않도록 함.
