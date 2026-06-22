# Project: Family Wealth & Cashflow Tracker Web App

## 1. Tech Stack & Architecture Constraints (Vanilla Stack)
* **Zero Build Tools:** Webpack, Babel, Vite 혹은 React, Vue, Flutter 등의 프레임워크를 일체 사용하지 않고, 순수 HTML5, CSS3, Vanilla JavaScript(ES6+)만 사용하여 구현할 것.
* **State Management Pattern:** 프레임워크가 없는 환경이므로 데이터와 UI 로직을 엄격히 분리해야 함. 전역 상태(Global State)를 관리하는 단일 데이터 객체(Store)를 두고, 데이터가 변경되면 연관된 UI 컴포넌트만 업데이트 되도록 중앙 집중식 데이터 흐름을 구현할 것 (무분별한 DOM 직접 조작 금지).
* **Modularization:** 코드가 하나의 파일에 비대해지는 것을 막기 위해 ES6 모듈(`import`/`export`)을 적극 활용하여 파일 구조를 `models/`, `store/`, `components/`, `utils/` 등으로 깔끔하게 분리할 것.
* **Data Storage:** 초기 프로토타이핑 및 로컬 데이터 보존을 위해 브라우저의 `LocalStorage`를 활용한 데이터 지속성(Persistence) 레이어를 구현하고, 향후 외부 API/DB 연동이 가능하도록 구조를 추상화할 것.
* **Charting:** 은퇴 시뮬레이션 및 자산 비중 시각화를 위한 차트 라이브러리(예: Chart.js 또는 ECharts)는 CDN 방식으로 가볍게 로드하여 사용할 것.

## 2. UI/UX & Design System
* **Design Reference:** 깃허브 `code-craftworks/child-tax-craft` 레포지토리의 디자인 시스템과 색상 체계를 100% 동일하게 차용할 것.
* **Component Styling:** 해당 레포지토리에서 사용된 타이포그래피, 버튼, 여백(Padding/Margin), 카드 UI 및 데이터 테이블 스타일을 그대로 이식하여 일관된 톤앤매너를 유지할 것.

## 3. Core Entities & Data Model
### A. Member Management (가족 구성원 관리)
* **CRUD 기능:** 자산 및 투자 주체를 분리하기 위한 인원 추가/삭제 기능 구현.
* **데이터 구조:** 본인(출생 연도 기준 나이 동기화), 배우자, 자녀 등 각 구성원별로 독립적인 포트폴리오를 가질 수 있도록 관계형 데이터 구조 설계.

### B. Asset & Liability Categorization (자산 상태표)
* **대차대조표(BS) 구조:** 모든 자산 항목은 **자산(Assets) / 현금(Cash) / 자본(Capital) / 부채(Liabilities)** 중 하나로 명확히 분류되어 총망라될 수 있도록 설계.
* **지원 항목 (Category/Enum):**
    * 주식 (국내, 해외 등)
    * 계좌 (IRP, ISA, 연금저축, 일반 예금/적금)
    * ETF (연금ETF 등)
    * 국민연금
    * 채권
    * 부동산
    * 차량 및 기타 실물 자산

## 4. Key Features
### A. Trading Journal (매매 일지)
* 종목별, 날짜별 매수/매도/배당 기록에 대한 수정, 삭제, 추가(CRUD) 기능 구현.
* **상태 동기화:** 매매 일지에 기록이 업데이트되면, 전역 데이터 스토어를 통해 전체 자산 총액, 현금 잔고, 구성원별 포트폴리오 비중 화면이 지연 없이 새로고침(Re-render)되도록 구현할 것.

### B. Cashflow & Retirement Projection (현금 흐름 및 은퇴 시뮬레이션)
* **타임라인 엔진:** 현재 시점부터 은퇴 시점을 지나 생애 주기 끝까지의 연도별/월별 타임라인 생성.
* **나이 동기화:** 기준 출생 연도를 바탕으로 매 연도별 사용자의 예상 나이를 함께 표기 (예: 2026년 46세 -> 2036년 56세).
* **수입/지출 모델링:** 연도별 및 월별 예상 수입/지출 금액 입력 기능.
* **은퇴 시뮬레이션 로직:**
    1. 설정한 예상 은퇴 시점(Target Retirement Age) 도달 타이밍 계산.
    2. 은퇴 후 필요한 **목표 현금 흐름(Required Cash Flow)** 산출.
    3. 누적된 자산(연금 수령액, 배당금, 부동산 임대 수익 등)에서 발생하는 **예상 현금 흐름(Generated Cash Flow)** 산출.
    4. 목표 현금 흐름 대비 발생 현금 흐름의 부족분/초과분을 직관적인 차트로 시각화.
