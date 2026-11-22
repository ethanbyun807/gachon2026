# 반도체 설계 자동화 (Semiconductor Design Automation)

> 202633475 변재민 (Ethan Byun)

## 목차

1. [반도체 설계 자동화란?](#1-반도체-설계-자동화란)
2. [간단한 역사](#2-간단한-역사)
3. [반도체 설계 흐름](#3-반도체-설계-흐름)
4. [주요 기업과 시장](#4-주요-기업과-시장)
5. [AI와 반도체 설계 자동화](#5-ai와-반도체-설계-자동화)
6. [오픈소스 EDA](#6-오픈소스-eda)
7. [참고자료](#7-참고자료)

---

## 1. 반도체 설계 자동화란?

**반도체 설계 자동화**는 반도체 칩(집적회로, IC)을 설계·분석·검증하는 전 과정을 소프트웨어 도구와 알고리즘으로 자동화하는 분야이다. 업계에서는 이 소프트웨어 분야를 **EDA(Electronic Design Automation, 전자설계자동화)** 라고 부른다. 

현대 반도체 칩에는 수십억 개의 소자가 들어가기 때문에, 사람이 손으로 설계하는 것은 사실상 불가능하며 자동화 도구가 필수이다. 이런 도구를 쓰면 칩을 더 빠르게 설계하고, 오류를 줄이고, **PPA(Performance·Power·Area)** , 즉 성능은 높이고 전력과 면적을 줄일 수 있다.

## 2. 간단한 역사

| 시기 | 내용 |
|---|---|
| 1950년대 | IBM이 700 시리즈 컴퓨터 문서화 과정에서 초기 설계 자동화를 시도 |
| 1971 | Calma가 GDS(Graphic Design System)를 Intel에 납품, 레이아웃을 컴퓨터로 편집 |
| 1970년대 중반 | 최초의 자동 배치·배선(Place and Route) 도구 등장 |
| 1984 | 첫 EDA 전시회가 Design Automation Conference(DAC)에서 열림 |
| 1986 | Gateway Design Automation이 하드웨어 기술 언어 Verilog 발표 |
| 1988 | SDA Systems와 ECAD가 합병해 Cadence 설립 |
| 2025 | Synopsys가 Ansys 인수 완료 (2025년 7월) |

## 3. 반도체 설계 흐름

디지털 칩은 대략 **명세(Spec) → RTL 설계 → RTL 검증 → PI → PD → Sign-off → Tape-out** 순서로 진행되며, 각 단계마다 EDA 도구가 쓰인다.

```
명세(Spec)         "어떤 칩을 만들지" 정하는 것
   ↓
RTL 설계            Verilog / SystemVerilog / VHDL
   ↓
RTL 검증            시뮬레이션, 형식 검증(Formal), 에뮬레이션
   ↓
  PI               RTL → 게이트 레벨 넷리스트 (+ DFT 삽입)
   ↓                → 합성 직후 STA로 타이밍 1차 확인
   ↓                → LEC로 RTL과 넷리스트의 논리 동등성 확인
  PD               Floorplan → Placement → CTS → Routing
   ↓                → 각 단계 후 STA 반복 (타이밍 위반 수정, 최적화)
   ↓                → 마무리 후 레이아웃 파일(GDSII 또는 OASIS) 출력
Sign-off           최종 STA·전력·기생 성분 추출, 출력된 레이아웃으로 DRC/LVS
   ↓                → 위반 시 ECO(부분 수정) 후 재출력, 모두 통과할 때까지 반복
Tape-out            최종 파일을 파운드리에 제출 → 마스크·웨이퍼 제작 시작
```

| 단계 | 하는 일 |
|---|---|
| RTL 설계·검증 | 레지스터 간 동작을 코드로 기술하고, 수많은 시나리오로 동작 확인 |
| PI(Physical Implementation) | RTL을 논리 게이트 넷리스트로 변환, 테스트 회로(DFT) 삽입. 합성 후 STA로 타이밍을 처음 확인하고, LEC로 RTL과 넷리스트가 논리적으로 같은지 검증 |
| PD(Physical Design) | 수십억 개 게이트의 위치를 정하고 금속 배선을 연결 (타이밍·전력·면적 만족). 배치, CTS, 배선 단계마다 STA를 반복하며 최적화하고, 마무리 후 레이아웃 파일(GDSII 또는 OASIS)을 출력 |
| Sign-off | 실제 배선 정보를 반영한 최종 STA와 전력, 출력된 레이아웃 기준의 물리 규칙(DRC/LVS)을 검증. 위반이 나오면 ECO 후 재출력해 반복 |
| Tape-out | 사인오프를 통과한 최종 GDSII를 파운드리에 제출. 이후 마스크·웨이퍼 제작이 시작되므로 되돌리기 어려움 |

> ECO : 이미 거의 완성된 설계를 처음부터 다시 하는 것이 아니라 필요한 부분만 최소한으로 고치는 수정 방식이다. 대표적인 종류로는 Timing ECO, Logical ECO, Physical Verification ECO 등이 있다.

## 4. 주요 기업과 시장

### 3대 기업

- **Synopsys** (미국): 논리 합성(Design Compiler), 배치·배선(Fusion Compiler), 타이밍 사인오프(PrimeTime) 등. 2025년 Ansys를 인수해 멀티피직스 해석까지 확장
- **Cadence** (미국): 배치·배선(Innovus), 아날로그·커스텀 설계, 검증 및 에뮬레이션 하드웨어 등
- **Siemens EDA** (독일, 구 Mentor Graphics): 물리 검증(Calibre) 등에 강점

이 세 회사가 시장의 약 85~88% 이상을 차지하는 과점 구조이다.

### 시장 규모

- 조사기관별 2025년 EDA 시장 규모 추정치는 크게 다르다. IMARC는 약 131억 달러, Fortune Business Insights는 약 196.5억 달러로 추정한다.
- 10년 이상 매년 성장을 이어 온 안정적인 산업으로 평가됨

## 5. AI와 반도체 설계 자동화

반도체 설계 자동화는 AI가 산업 현장에 가장 깊게 들어온 분야 중 하나로 꼽힌다. 흐름을 단계로 나누면 다음과 같다.

| 단계 | 설명 | 예시 |
|---|---|---|
| 점(point) 최적화 AI | 특정 구간만 잘 찾아주는 내비게이션 역할 (강화학습·머신러닝) | Synopsys DSO.ai (2020), Google AlphaChip(칩 배치) |
| 생성형 AI 보조 | 말로 물어보면 코드를 짜 주는 조수 역할 (LLM이 RTL 작성, 질의응답, 스크립트 생성 지원) | Synopsys.ai (2023), Chip-Chat 등 연구 |
| 에이전틱 AI | AI가 도구를 직접 호출해 설계 흐름의 일부를 수행 | Cadence ChipStack, Synopsys AgentEngineer·AutoPilot, Siemens Fuse EDA AI Agent |
| 완전 자율 설계 | 사람은 의도만 정의, AI가 전체 흐름 수행 | 아직 연구·비전 단계 |

지금은 세 번째 단계가 막 시작된 시점이다. 다만 LLM 기반 에이전트가 사람 개입 없이 실제 산업용 칩을 끝까지 설계해 테이프아웃한 사례는 아직 보고 되지 않았고, 남은 과제로는 환각(Hallucination), 학습 데이터 부족, 설계 IP 보안 등이 있다. 

## 6. 오픈소스 EDA

상용 도구는 고가이고 접근이 어렵기 때문에, 학습·연구·소규모 설계를 위한 오픈소스 흐름이 발전해 왔다.

| 프로젝트 | 설명 |
|---|---|
| [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD) | DARPA IDEA 프로그램에서 시작. "사람 개입 없이 24시간 안에 RTL→GDS"를 목표로 하는 오픈소스 디지털 설계 도구. 600건 이상의 테이프아웃 가능 설계에 사용됨 (최대 12nm급) |
| OpenROAD-flow-scripts (ORFS) | OpenROAD 기반의 RTL→GDSII 흐름 스크립트 |
| OpenLane | Yosys와 OpenROAD 등을 묶은 자동화 RTL→GDSII 흐름 (SkyWater 130nm, GF 180nm 지원) |
| Yosys + ABC | 오픈소스 논리 합성 |
| KLayout, Magic, Netgen | 레이아웃 보기·DRC/LVS 검증 |
| Qflow, Alliance/Coriolis, iEDA | 그 외 오픈소스 RTL→GDS 흐름 |

공개 PDK(Process Design Kit): SkyWater SKY130(130nm), GF180(180nm), Nangate45(45nm), ASAP7(7nm급 예측 모델)

## 7. 참고자료

- [Electronic design automation - Wikipedia](https://en.wikipedia.org/wiki/Electronic_design_automation)
- [Design flow (EDA) - Wikipedia](https://en.wikipedia.org/wiki/Design_flow_(EDA))
- [What is EDA? - Synopsys](https://www.synopsys.com/glossary/what-is-electronic-design-automation.html)
- [The EDA Primer: From RTL to Silicon - SemiAnalysis](https://newsletter.semianalysis.com/p/the-eda-primer-from-rtl-to-silicon)
- [EDA Market Primer - SemiAnalysis](https://newsletter.semianalysis.com/p/eda-market-primer)
- [The state of agentic AI in chip design tools in 2026 - Tom's Hardware](https://www.tomshardware.com/tech-industry/semiconductors/the-state-of-agentic-ai-in-chip-design-tools-in-2026-cadence-synopsys-and-siemens-all-pitch-autonomous-engineers)
- [The Dawn of Agentic EDA: A Survey of Autonomous Digital Chip Design - arXiv:2512.23189](https://arxiv.org/abs/2512.23189)
- [Self-verifying EDA AI agents - Siemens](https://blogs.sw.siemens.com/cicv/2026/07/29/self-verifying-eda-ai-agents/)
- [The OpenROAD Project - GitHub](https://github.com/The-OpenROAD-Project/OpenROAD)
- [Toward an Open-Source Digital Flow: First Learnings from the OpenROAD Project](https://par.nsf.gov/servlets/purl/10171025)
- [Electronic Design Automation Market Report - IMARC](https://www.giikorea.co.kr/report/imarc2049282-electronic-design-automation-market-report-by.html)
- [Electronic Design Automation Market - Fortune Business Insights](https://www.fortunebusinessinsights.com/ko/electronic-design-automation-market-117148)