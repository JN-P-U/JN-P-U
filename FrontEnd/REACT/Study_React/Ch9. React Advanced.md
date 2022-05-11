<h1>Ch9. React Advanced</h1>

1. Optimizing Performance

- 필요할때만 렌더한다.
- Reconciliation
  - render 전후의 일치 여부를 판단하는 규칙
  - 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 생성
  - 개발자가 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시할 수 있음

2. React.createPortal

3. React.forwardRef