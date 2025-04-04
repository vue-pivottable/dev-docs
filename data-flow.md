
# Vue Pivottable 다이어그램

## 컴포넌트 트리구조

```mermaid
graph TD
  %% 색상 정의
  classDef props fill:#d4f1f9,stroke:#000,stroke-width:1px,color:#000
  classDef state fill:#ffe6cc,stroke:#000,stroke-width:1px,color:#000
  classDef computed fill:#e6ffcc,stroke:#000,stroke-width:1px,color:#000
  classDef event fill:#ffe6e6,stroke:#000,stroke-width:1px,color:#000
  classDef component fill:#fff,stroke:#000,stroke-width:2px,color:#000
  classDef warnning fill:red,color:#fff

  A[VPivottableUI]:::component --> B[VDragnDropCell]:::component --> C[VdraggableAttribute]:::component --> D[VFilterBox]:::component
  A --> E[VRendererCell]:::component
  A --> G[VAggregatorCell]:::component
  A --> H[VPivottable]:::component --> I[TableRenderer]:::component

```

## VPivottableUI 컴포넌트 데이터 흐름도

### VRendererCell

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
    P1[Props: renderers]:::props --> P1-1[Computed: renderItems]:::computed --> P1-2
    P2[Props: rendererName]:::props --> P2-1

    subgraph B[VRendererCell]
      P1-2[Props: renderItems]:::props --> P1-3[Computed: rendererNames]:::computed --> P1-4
      P2-1[Props: rendererName]:::props --> P2-2

      subgraph C[VDropdown]
        P1-4[Props: options]:::props
        P2-2[Props: rendererName]:::warnning --> P2-3[State: value]:::state 
      end
    end
  end
```

### VAggregatorCell > VDropdown(Aggregator)

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
    P1[Props: aggregators]:::props --> P1-1[Computed: aggregatorItems]:::computed --> P1-2
    P2[Props: aggregatorName]:::props --> P2-1
    P3[Props: rowOrder]:::props --> P3-1
    P4[Props: colOrder]:::props --> P4-1

    subgraph B[VAggregatorCell]
      P1-2[Props: aggregatorItems]:::props --> P1-3[Computed: aggregatorNames]:::computed --> P1-4
      P2-1[Props: aggregatorName]:::props --> P2-2
      P3-1[Props: rowOrder]:::props --> P3-2
      P4-1[Props: colOrder]:::props --> P4-2

      subgraph C[VDropdown]
        P1-4[Props: options]:::props
        P2-2[Props: aggregatorName]:::warnning --> P2-3[State: value]:::state
        P3-2[Props: rowOrder]:::props
        P4-2[Props: colOrder]:::props
      end
    end
  end
```

### VAggregatorCell > VDropdown(Vals)

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
    P1-1[Props: allAttributes]:::props --> P1-2
    P1-2[Computed: attributeNames]:::computed --> P1-3
    P2-1[Props: hiddenAttributes]:::props --> P1-2
    P3-1[Props: hiddenFromAggregators]:::props --> P3-2
    P4-1[Props: vals]:::props --> P4-2

    subgraph B[VAggregatorCell]
      P1-3[Props: attributeNames]:::props --> P1-4
      P3-2[Props: hiddenFromAggregators]:::props --> P1-4
      P1-4[Computed: valsOptions]:::warnning --> P1-5
      P4-2[Props: vals]:::props --> P4-3

      subgraph C[VDropdown]
        P1-5[Props: options]:::props
        P4-3[Props: vals]:::warnning --> P4-4[State: value]:::state

      end
    end
  end

```

### VDragnDropCell > VDraggableAttrubute > VFilterBox

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
    P1[Props: locale]:::props --> AA1
    P2[Props: languagePack]:::props --> AA1

    subgraph AA[Provide]
      AA1[Computed: localeStrings]:::computed -->|Provide| P1-2
      AA2[maxZIndex]
      AA3[sorter]
    end

    

    P3[Props: cols]:::props --> P3-1[Computed: colAttrs]:::computed --> P3-2
    P4[Props: rows]:::props --> P4-1[Computed: rowAttrs]:::computed --> P3-2

    P5[State: allAttributesWithFilters]:::state --> P5-1

    P6[Props: valueFilter]:::props --> P6-1


    subgraph B[VDragnDropCell]
      P3-2[Props: attributes]:::warnning --> P3-3
      P5-1[Props: attributes]:::warnning --> P5-2
      P6-1[Props: valueFilter]:::props --> P6-2

      subgraph C[VDraggableAttribute]
        P3-3[Props: attributeName]:::props --> P3-4
        P5-2[Props: attributeValues]:::props --> P5-3
        P6-2[Props: unselecteFilterValues]:::props --> P6-3

        subgraph D[VFilterBox]
          P1-2[State: localeStrings]:::state
          P3-4[Props: filterBoxKey]:::props
          P5-3[Props: filterBoxValues]:::props
          P6-3[Props: unselecteFilterValues]:::props

        end
      end
    end
  end
```
