
# VRendererCell

## Diagram

```mermaid
graph TD
  %% 색상 정의
  classDef props fill:#d4f1f9,stroke:#000,stroke-width:1px,color:#000
  classDef state fill:#ffe6cc,stroke:#000,stroke-width:1px,color:#000
  classDef computed fill:#e6ffcc,stroke:#000,stroke-width:1px,color:#000
  classDef event fill:#ffe6e6,stroke:#000,stroke-width:1px,color:#000
  classDef component fill:#fff,stroke:#000,stroke-width:2px,color:#000
  classDef warnning fill:red,color:#fff
  
  subgraph A[VPivottableUI]
    A-P1[Props: renderers]:::props
    A-P2[Props: rendererName]:::props
    A-C1[Computed: renderItems]:::computed
    A-E1[Method: updateRenderName]:::event

    subgraph B[VRendererCell]
      B-P1[Props: renderItems]:::props
      B-C1[Computed: rendererNames]:::computed
      B-P2[Props: rendererName]:::props
      B-E1[Emit: update:renderName]:::event

      subgraph C[VDropdown]
        C-P1[Props: options]:::props
        C-P2[Props: value]:::props
        C-S1[State: valueModel]:::state 
        C-E1[Emit: update:value]:::event
      end
    end

    %% 데이터 흐름
    A-P1 --> A-C1 --> B-P1 --> B-C1 --> C-P1
    A-P2 --> B-P2 --> C-P2 --> C-S1

    %% 이벤트 흐름
    C-E1 --> B-E1 --> A-E1
  end
```

## Description

| 컴포넌트 | 유형 | 이름 | 설명 |
|---------|------|------|------|
| VPivottableUI | Props | renderers | 개발자가 커스텀으로 제공하는 렌더러 목록 객체 배열 |
| | Props | rendererName | 설정된 렌더러타입의 이름 |
| | Computed | renderItems | renderers props 또는 기본 테이블 렌더러 목록 객체 배열 |
| | Method | updateRenderName | 렌더러타입의 이름을 업데이트 하는 메소드 |
| VRendererCell | Props | renderItems | VPivottableUI로부터 전달받은 렌더러 아이템 목록 |
| | Props | rendererName | 현재 선택된 렌더러 이름 |
| | Computed | rendererNames | renderItems를 기반으로 드롭다운에 표시할 렌더러 이름 목록 |
| | Emit | update:renderName | 렌더러 이름이 변경될 때 상위 컴포넌트로 이벤트 전달 |
| VDropdown | Props | options | 드롭다운에 표시될 옵션 목록 |
| | Props | value | 현재 선택된 값 |
| | State | valueModel | value props를 기반으로 내부 상태 관리 |
| | Emit | update:value | 값이 변경될 때 상위 컴포넌트로 이벤트 전달 |
