[Desgining APIs with Clean Architecture](https://theprodev.medium.com/desgining-apis-with-clean-architecture-2fedff1b79cb)

Clean Architecture opposed to Three Tier Architecture

Clean Architecture 特性
- 核心是能夠獨立於基礎架構的其餘部分測試和開發業務應用程序
- 業務規則構成了應用程序的實際核心
- 可以防止單個軟件解決方案影響整個流程，從而增加和阻礙業務流程
  - 在復雜的軟件項目中，進一步的開發永遠不會完成
- 基礎設施的組件可以在以後定義，也可以交換或替換

Clean Architecture 架構
- 不以Tiers或Steps的形式定義它的層，而是定義為依賴環
- 最外面的環更容易受到環境變化的影響，而最裡面的環幾乎不受環境變化的影響
- 在最內層，域層完全獨立於數據模型如何加載和持久化到系統中
- 每個單獨的基礎設施和其他外部影響都被設計為適配器，並附加到內層暴露的端口以供其使用

設計 API
- API（REST 和其他）必須附加到它的基本組件——模式(schema)和數據存儲(data store)。我們可以將模式視為我們的表示層
- 解耦合
    - 根據消費者應用程序和 API 客戶端要求進行更改,不總是需要更改我們的業務邏輯甚至實體模型
    - 內部算法的任何新變化都不需要對schema或entity進行任何修改
    - 數據庫系統的任何升級最多只能影響實體定義


Example:
https://github.com/0xTheProDev/fastapi-clean-example
