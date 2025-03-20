# Vue3 Pivottable 컴포넌트 구조 및 마이그레이션 설계

Vue Pivottable은 jQuery 기반의 PivotTable.js를 Vue 환경으로 포팅한 라이브러리로, 피벗 테이블과 차트를 유연하게 생성할 수 있는 방법을 제공합니다.

## 컴포넌트 계층 구조 (AS-IS)

```bash
vue-pivottable/
├── VuePivottable (메인 테이블 컴포넌트)
│   ├── createPivottable() - 피벗 테이블 인스턴스 생성
│   ├── createWrapperContainer() - 외부 컨테이너 생성
│   └── TableRenderer - 실제 피벗 테이블 렌더링
│       ├── Table - 기본 테이블 렌더러
│       ├── Table Heatmap - 히트맵 형식의 테이블 렌더러
│       ├── Table Col Heatmap - 열 기반 히트맵 테이블 렌더러
│       ├── Table Row Heatmap - 행 기반 히트맵 테이블 렌더러
│       └── Export Table TSV - TSV 형식 내보내기 렌더러
│
└── VuePivottableUi (드래그-드롭 기능을 갖춘 인터랙티브 UI)
    ├── makeDnDCell() - 드래그 가능한 셀 생성 함수
    ├── rendererCell() - 렌더러 선택 셀 생성 함수
    ├── aggregatorCell() - 집계 함수 선택 셀 생성 함수
    ├── outputCell() - 결과 출력 셀 생성 함수
    ├── DraggableAttribute - 개별 드래그 가능한 필드
    │   ├── toggleFilterBox() - 필터 박스 토글 함수
    │   ├── getFilterBox() - 필터 박스 생성 함수
    │   └── FilterBox - 값 필터링용
    ├── Dropdown - 집계 함수와 렌더러 선택기
    └── VuePivottable - 내장된 테이블 컴포넌트
```

### 핵심 컴포넌트 및 함수

1. **VuePivottable** (`src/Pivottable.js`)
   - UI 컨트롤이 없는 기본 피벗 테이블 컴포넌트
   - `createPivottable()`: 피벗 테이블 인스턴스를 생성하는 메서드
   - `createWrapperContainer()`: 테이블을 감싸는 컨테이너를 생성하는 메서드

2. **VuePivottableUi** (`src/PivottableUi.js`)
   - 드래그 앤 드롭 필드 컨트롤이 있는 인터랙티브 UI
   - `makeDnDCell()`: 드래그 가능한 셀을 생성하는 메서드
   - `rendererCell()`: 렌더러 선택 UI를 생성하는 메서드
   - `aggregatorCell()`: 집계 함수 선택 UI를 생성하는 메서드
   - `outputCell()`: 결과 테이블 출력 영역을 생성하는 메서드

3. **TableRenderer** (`src/TableRenderer.js`)
   - `makeRenderer()`: 다양한 유형의 테이블 렌더러를 생성하는 함수
   - 제공되는 렌더러 유형:
     - `Table`: 기본 테이블 형식
     - `Table Heatmap`: 전체 히트맵 테이블
     - `Table Col Heatmap`: 열 기반 히트맵 테이블
     - `Table Row Heatmap`: 행 기반 히트맵 테이블
     - `Export Table TSV`: TSV 형식으로 내보내기

4. **DraggableAttribute** (`src/DraggableAttribute.js`)
   - `toggleFilterBox()`: 필터 박스를 표시하거나 숨기는 메서드
   - `getFilterBox()`: 필터 UI를 생성하는 메서드

5. **Dropdown** (`src/Dropdown.js`)
   - 렌더러와 집계 함수 선택을 위한 간단한 드롭다운 컴포넌트

### 데이터 처리 핵심 클래스

1. **PivotData** (`src/helper/utils.js`)
   - 피벗 테이블 데이터를 계산하는 핵심 클래스
   - `getAggregator()`: 특정 행/열 조합에 대한 집계 결과를 가져오는 메서드
   - `getColKeys()`, `getRowKeys()`: 테이블 구조를 위한 키 배열을 가져오는 메서드

## 컴포넌트 계층 구조 (TO-BE)

```bash
vue3-pivottable/
├── pivottable/
│   ├── VPivottable.vue - 메인 피벗 테이블 컴포넌트 (AS-IS: VuePivottable)
│   │   ├── VPivottableHeader.vue - 테이블 헤더 컴포넌트 (AS-IS: 내부 헤더 렌더링 로직)
│   │   │   ├── VPivottableHeaderRows.vue - 헤더 행 컴포넌트 (AS-IS: 내부 로직)
│   │   │   └── VPivottableHeaderColTotals.vue - 열 합계 헤더 (AS-IS: 내부 로직)
│   │   └── VPivottableBody.vue - 테이블 바디 컴포넌트 (AS-IS: 내부 바디 렌더링 로직)
│   │       ├── VPivottableBodyRows.vue - 테이블 행 컴포넌트 (AS-IS: 내부 로직)
│   │       └── VPivottableBodyRowsTotalRow.vue - 행 합계 컴포넌트 (AS-IS: 내부 로직)
│   ├── renderer/
│   │   └── VTableRenderer.vue - 통합 테이블 렌더러 컴포넌트 (AS-IS: TableRenderer 및 makeRenderer() 함수)
│   ├── composables/
│   │   ├── usePivotData.js - PivotData 클래스를 Vue3 반응형으로 래핑 (AS-IS: PivotData 클래스)
│   │   ├── useSorting.js - 정렬 관련 기능 래핑 (AS-IS: naturalSort, getSort, sortAs 함수)
│   │   ├── useValueFormatting.js - 값 포맷팅 기능 래핑 (AS-IS: numberFormat 함수)
│   │   ├── useAggregators.js - 집계 함수 래핑 (AS-IS: aggregators 및 aggregatorTemplates)
│   │   └── useFilterBox.js - 필터 박스 기능 래핑 (AS-IS: DraggableAttribute의 필터 관련 로직)
│   └── helper/
│       └── utils.js - 기존 유틸리티 함수 유지 (변경 없음)
│   
└── pivottable-ui/
    └── VPivottableUi.vue - 드래그-드롭 UI 제공 메인 컴포넌트 (AS-IS: VuePivottableUi)
        ├── VRendererCell.vue - 렌더러 선택 UI (AS-IS: rendererCell() 함수)
        │   └── VDropdown.vue - 드롭다운 컴포넌트 (AS-IS: Dropdown 컴포넌트)
        ├── VDragAndDropCell.vue - 드래그&드롭 영역 (AS-IS: makeDnDCell() 함수)
        │   ├── Draggable (외부 컴포넌트) - Vue3용 draggable (AS-IS: vuedraggable 컴포넌트)
        │   └── VDraggableAttribute.vue - 드래그 가능한 필드 컴포넌트 (AS-IS: DraggableAttribute)
        │       └── VFilterBox.vue - 필터 컴포넌트 (AS-IS: getFilterBox() 함수 결과)
        ├── VSortControls.vue - 정렬 컨트롤 (AS-IS: 내부 정렬 아이콘 로직)
        ├── VAggregatorCell.vue - 집계 함수 선택 UI (AS-IS: aggregatorCell() 함수)
        │   └── VDropdown.vue - 드롭다운 컴포넌트 (AS-IS: Dropdown 컴포넌트)
        └── VPivottable.vue - 내장된 테이블 컴포넌트 (AS-IS: 내부 VuePivottable 사용)

```

### 핵심 설계 원칙

1. 기존 렌더러 구조 유지: 하나의 렌더러 컴포넌트로 여러 테이블 형식을 처리하는 기존 접근 방식 유지
2. Vue3 컴포넌트로 변환: 렌더 함수 방식의 구현을 Vue3의 template 및 <script setup> 방식으로 전환
3. 기존 utils.js 유지: 핵심 계산 로직과 유틸리티 함수는 변경 없이 유지
4. 컴포저블 활용: 상태 관리 및 로직을 Vue3 Composition API로 래핑
5. 점진적 컴포넌트 세분화: 성능 요구사항에 따라 테이블을 더 작은 컴포넌트로 점진적으로 분리 가능

### 핵심 컴포넌트 및 함수

1. **VPivottable.vue** (`pivottable/VPivottable.vue`)
   - Vue3 template 구문으로 구현된 기본 피벗 테이블 컴포넌트

2. **VPivottableUi.vue** (`pivottable-ui/VPivottableUi.vue`)
   - 드래그 앤 드롭을 지원하는 인터랙티브 UI
   - `emit()`: 이벤트 핸들링 (drag, drop, filter, change 등)

3. **VTableRenderer.vue** (`pivottable/renderer/VTableRenderer.vue`)
   - 다양한 테이블 렌더링 모드를 지원하는 통합 컴포넌트
   - `props.heatmapMode`: 히트맵 표시 모드 설정 ('full', 'col', 'row')
   - `setup()`: 렌더러 초기화 및 상태 관리
   - `useHeatmapRenderer()`: 히트맵 시각화 로직 (히트맵 색상 계산)

4. **VDraggableAttribute.vue** (`pivottable-ui/VDraggableAttribute.vue`)
   - 드래그 가능한 속성 필드 컴포넌트
   - `toggleFilter()`: 필터 박스 표시 제어
   - `useFilterState()`: 필터 상태 관리

5. **VDropdown.vue** (`pivottable-ui/VDropdown.vue`)
   - 렌더러 및 집계 함수 선택을 위한 드롭다운 컴포넌트
   - `emit('update:modelValue')`: Vue3의 v-model 지원

### Composables (TO-BE)

1. **usePivotData.js** (`pivottable/composables/usePivotData.js`)
   - 피벗 테이블 데이터 계산 로직을 Vue3 반응형으로 래핑
   - `pivotData`: `ref()`로 관리되는 피벗 데이터
   - `computePivotData()`: 데이터 계산 함수
   - `getAggregator()`, `getColKeys()`, `getRowKeys()`: 기존 PivotData 메서드의 반응형 래퍼

2. **useSorting.js** (`pivottable/composables/useSorting.js`)
   - 정렬 관련 함수를 Vue3 방식으로 제공
   - `sortAs()`, `naturalSort()`, `getSort()`: 기존 정렬 로직 래핑
   - `createSorter()`: 정렬 함수 생성 헬퍼

3. **useValueFormatting.js** (`pivottable/composables/useValueFormatting.js`)
   - 값 포맷팅 로직을 Vue3 방식으로 제공
   - `formatValue()`: 값 포맷팅 함수
   - `createFormatter()`: 사용자 정의 포매터 생성 헬퍼

4. **useAggregators.js** (`pivottable/composables/useAggregators.js`)
   - 집계 함수를 Vue3 방식으로 래핑
   - `getAggregator()`: 선택된 집계 함수 반환
   - `aggregatorTemplates`: 기존 집계 함수 템플릿을 반응형으로 관리

5. **useFilterBox.js** (`pivottable/composables/useFilterBox.js`)
   - 필터 박스 기능 래핑
   - `filterState`: 필터 상태 관리 ref
   - `toggleFilter()`: 필터 활성화/비활성화
   - `addFilterValue()`, `removeFilterValue()`: 필터 값 조작
   - `matchFilter()`: 필터 매칭 확인

### 주요 데이터 흐름 (TO-BE)

1. **상태 관리**
   - `pivotData`: 피벗 데이터의 계산 및 구조를 관리하는 중앙 상태
   - `filterState`: 사용자가 적용한 필터 설정을 관리하는 상태
   - `dragState`: 드래그 앤 드롭 상태와 정보를 관리

2. **이벤트 처리**
   - Vue3의 `emits` 옵션을 사용하여 명시적인 이벤트 핸들링
   - 단방향 데이터 흐름: 부모 컴포넌트에 이벤트 발생, 상태 업데이트

3. **반응형 업데이트**
   - Composition API의 `watch`, `computed`를 활용한 반응형 데이터 흐름
   - Render 함수 대신 Vue3 템플릿 및 `v-if`, `v-for` 활용

### 마이그레이션 접근 방식

1. **점진적 마이그레이션**
   - 첫 단계: 핵심 기능을 Vue3 컴포넌트로 변환
   - 두 번째 단계: 모든 렌더링 함수를 템플릿으로 전환
   - 세 번째 단계: 성능 최적화를 위한 컴포넌트 세분화

2. **API 호환성**
   - 기존 props 구조 유지
   - 새로운 이벤트 API 추가 (기존 방식과 호환성 유지)

3. **렌더링 최적화**
   - 가상 스크롤링 지원 추가
   - 큰 데이터셋에 대한 렌더링 성능 개선
   - `<Suspense>`를 활용한 비동기 데이터 로딩 지원
