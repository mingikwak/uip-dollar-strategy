# uip-dollar-strategy
uip-dollar-strategy/
├── data/                 # FRED, ECOS, yfinance 수집 데이터
├── notebooks/
│   ├── 01_data_prep.ipynb        # 데이터 병합 및 정상성 검정(ADF)
│   ├── 02_uip_regression.ipynb   # UIP 회귀분석 및 금리 역전기 분석
│   └── 03_dollar_backtest.ipynb  # 원화예금 vs 환노출/환헤지 채권 백테스팅
├── src/                  # 데이터 로더 및 백테스팅 함수 모듈화
└── README.md             # 영문 요약 + 핵심 차트 2~3장 (회귀 결과, 누적 자산 곡선)
