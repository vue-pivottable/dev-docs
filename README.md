# 피벗테이블 개발 문서

> _작성중인 내용으로 정확하지 않을 수 있습니다_

- [Vue Pivottable 라이브러리 구조 분석](./vue-pivottable-as-is.md)
- [Vue3 Pivottable 컴포넌트 구조 및 마이그레이션 설계](./vue-pivottable-migration-design-spec.md)
- [컴포넌트 트리구조](./diagram/component.md)
- [데이터 흐름 다이어그램: VAggregatorCell](./diagram/VAggregatorCell.md)
- [데이터 흐름 다이어그램: VDraggableAttribute](./diagram/VDraggableAttribute.md)
- [데이터 흐름 다이어그램: VRendererCell](./diagram/VRendererCell.md)

## 컴포넌트 구조

![001](./images/vue-pivottable-cmp-1.png)
![002](./images/vue-pivottable-cmp-2.png)

|No.|Name               |Description        |
|---|-------------------|-------------------|
|1  |VAggregatorCell    |집계함수와 그룹화할 속성을 선택하고 데이터를 정렬합니다.|
|2  |VRendererCell      |렌더러 타입을 선택하는 셀입니다.|
|3  |VDragnDropCell     |드래그 앤 드랍이 가능한 셀입니다.|
|4  |VPivottable        |선택한 렌더러 또는 제공받은 렌더러를 감싸는 영역입니다.|
|5  |TableRenderer      |내장된 테이블 렌더러 컴포넌트입니다.|
|6  |VDraggableAttribute|DnD셀 영역에 위치한 속성들입니다.|
|7  |VFilterBox         |선택한 속성의 데이터 값들을 필터링합니다.|
|-  |VPivottableUi      |전체를 포괄하는 피벗테이블 사용자 인터페이스 영역입니다.|
|-  |VDropdown          |(1)과 (2)에서 재사용하는 드랍다운 컴포넌트입니다.|

<!-- ## vue-pivottable 컴포넌트별 기능 정의표

### VAggregatorCell

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 데이터 집계 | 다양한 집계 방식으로 데이터 계산 | aggregatorName, vals |
| 정렬 | 행과 열 데이터 정렬 방식 지정 | rowOrder, colOrder | -->

<!-- ### VRendererCell

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 렌더러 선택 | 다양한 테이블 표시 방식 선택 | rendererName, renderers | -->

<!-- ### VDragnDropCell

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------| -->

<!-- ### VPivottable

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 피벗 테이블 기본 표시 | 데이터를 피벗 테이블 형식으로 렌더링 | data, rows, cols |
| 행/열 합계 | 행과 열의 합계 값 표시 여부 | rowTotal, colTotal |
| 테이블 크기 제한 | 테이블의 최대 너비 설정 | tableMaxWidth | -->

<!-- ### TableRenderer

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 테이블 렌더링 | 기본 테이블 형식으로 데이터 표시 | render |
| 헤더 셀 병합 | 계층적 헤더를 위한 셀 병합 | spanSize |
| 히트맵 모드 | 색상 강도를 통한 데이터 시각화 | heatmapMode, tableColorScaleGenerator |
| 클릭 이벤트 처리 | 셀 클릭 시 사용자 정의 이벤트 처리 | getClickHandler, tableOptions |
| TSV 내보내기 | 데이터를 탭 구분 텍스트로 내보내기 | TSVExportRenderer | -->

<!-- ### VDraggableAttribute

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 속성 드래그 기능 | 속성을 드래그 가능한 요소로 렌더링 | draggable, sortable |
| 필터 팝업 | 속성 값 필터링 UI 표시 | getFilterBox, open | -->

<!-- ### VFilterBox

| 값 필터링 | 특정 속성 값을 포함/제외 | toggleValue, addValuesToFilter, removeValuesFromFilter |
| 필터 검색 | 속성 값 목록 검색 | filterText, matchesFilter |
| 전체 선택/해제 | 모든 값 선택/해제 옵션 | selectAll, selectNone | -->

<!-- 
### VPivottableUi

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 드래그 앤 드롭 인터페이스 | 속성을 행/열/값 영역으로 드래그하여 구성 변경 | - |
| 속성 필터링 | 각 속성에 대한 값 필터 설정 UI | valueFilter, updateValueFilter |
| 렌더러 선택 UI | 다양한 테이블 표시 방식을 UI로 선택 | rendererCell |
| 집계 함수 선택 UI | 집계 함수를 UI로 선택 | aggregatorCell |
| 속성 관리 영역 | 미사용/사용 속성 관리 영역 | unusedAttrs, rowAttrs, colAttrs |
| 속성 숨김 설정 | 특정 속성을 피벗 테이블에서 숨김 | hiddenAttributes |
| 드래그 앤 드롭 제한 | 특정 속성의 드래그 앤 드롭 제한 | hiddenFromDragDrop, disabledFromDragDrop, sortonlyFromDragDrop |
| 설정 동기화 | 피벗 테이블 설정 변경 감지 및 전파 | init, propsData, propUpdater | -->

<!-- ### VDropdown

| 기능 | 설명 | 관련 속성/메서드 |
|------|------|--------------|
| 드롭다운 선택 | 선택 옵션을 드롭다운으로 표시 | values, value |
| 값 변경 감지 | 선택 값 변경 시 이벤트 발생 | handleChange | -->

<!-- 
## 이벤트 및 상호작용

| 기능 | 설명 | 관련 이벤트/메서드 |
|------|------|--------------|
| 설정 변경 감지 | 피벗 테이블 설정 변경 감지 | onRefresh |
| 필터 업데이트 | 값 필터 변경 감지 | update:valueFilter |
| 드래그 앤 드롭 이벤트 | 속성 드래그 및 드롭 이벤트 | dragged:unused, dropped:unused, dragged:cols, dropped:cols, dragged:rows, dropped:rows |
| 필터 상자 제어 | 필터 상자 열기/닫기 이벤트 | open:filterbox, moveToTop:filterbox |
| 에러 처리 | 렌더링/계산 오류 처리 | renderError, computeError, uiRenderError | -->
