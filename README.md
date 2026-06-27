# Dataset_for_Actuary

「예측 모델링, Vibe Coding과 AI 학습으로」 (2026.7.9~10, 한국보험계리사회) 강의 실습용 데이터셋.

출처: SOA Predictive Analytics(PA) 기출 데이터 (2019·2020). 강의 실습 목적으로 타깃 가공·정리함.

---

## 📂 파일

| 파일 | 문제 유형 | 타깃 | 행×열 | 비고 |
|---|---|---|---|---|
| `readmission.csv` | 분류 (이진) | `readmit_30d` (1/0) | 10,000 × 13 | 30일내 재입원 예측. 불균형(양성 11.3%) |
| `bike_rentals.csv` | 회귀 (count) | `bikes_per_hour` | 17,376 × 10 | 시간당 자전거 대여수 예측 |

---

## Colab에서 불러오기 (R 런타임)

> Colab 상단 **런타임 → 런타임 유형 변경 → R** 선택 후 실행.
> R 런타임은 구글 드라이브 마운트가 안 되므로, 아래처럼 URL에서 직접 받습니다.

```r
# 재입원 (분류)
download.file("https://raw.githubusercontent.com/kimsunki12/Dataset_for_Actuary/main/readmission.csv", "readmission.csv")
readmit <- read.csv("readmission.csv", stringsAsFactors = TRUE, na.strings = "?")

# 자전거 (회귀)
download.file("https://raw.githubusercontent.com/kimsunki12/Dataset_for_Actuary/main/bike_rentals.csv", "bike_rentals.csv")
bike <- read.csv("bike_rentals.csv")
```

`na.strings = "?"` — 재입원 데이터의 결측 표시가 `?`라서 이렇게 읽으면 자동으로 `NA` 처리됩니다.

---

## 1. readmission.csv (분류)

당뇨 환자의 **30일내 재입원 여부**를 예측. 원본 3분류(`<30`/`>30`/`NO`)를 이진화: `<30` → **1**, 나머지 → **0**. 양성 비율 11.3%로, **불균형 데이터 처리(언더/오버 샘플링) 실습에 적합.**

| 변수 | 유형 | 설명 |
|---|---|---|
| `race` | 범주 | 인종 (일부 결측 `?`) |
| `gender` | 범주 | 성별 (Female/Male, 소수 Unknown) |
| `age` | 범주 | 연령대 구간 `[0-10)`~`[90-100)` |
| `weight` | 범주 | 체중 구간. **약 97% 결측(`?`)** → 결측치 처리 실습용 |
| `admit_type_id` | 범주 | 입원 유형 (1/2/3) |
| `num_labs` | 수치 | 검사 횟수 |
| `num_procs` | 수치 | 시술 횟수 |
| `num_meds` | 수치 | 투약 종류 수 |
| `num_ip` | 수치 | 입원 이력 수 |
| `num_diags` | 수치 | 진단 수 |
| `metformin` | 범주 | 메트포민 처방 (No/Steady/Up/Down) |
| `insulin` | 범주 | 인슐린 처방 (No/Steady/Up/Down) |
| **`readmit_30d`** | **타깃** | **1 = 30일내 재입원, 0 = 아님** |

활용 세션: ② EDA · ③ 데이터 변환(결측·불균형) · ⑤ 분류 트리 · ⑥ 로지스틱 GLM · ⑧ ROC/AUC

---

## 2. bike_rentals.csv (회귀)

시간대·날씨 조건으로 **시간당 자전거 대여 수**를 예측. count 데이터라 **포아송 GLM·offset 실습에 적합.**

| 변수 | 유형 | 설명 |
|---|---|---|
| `season` | 범주 | 계절 (1~4) |
| `year` | 범주 | 연도 (0=1년차, 1=2년차) |
| `hour` | 수치 | 시각 (0~23) |
| `holiday` | 범주 | 공휴일 여부 (0/1) |
| `weekday` | 범주 | 요일 (0~6) |
| `weathersit` | 범주 | 날씨 상태 (1=맑음 ~ 3=악천후) |
| `temp` | 수치 | 정규화 기온 (0~1) |
| `humidity` | 수치 | 정규화 습도 (0~1) |
| `windspeed` | 수치 | 정규화 풍속 (0~1) |
| **`bikes_per_hour`** | **타깃** | **시간당 대여 수 (1~977)** |

활용 세션: ② EDA · ④ 비지도 학습 · ⑤ 회귀 트리 · ⑥⑦ 포아송 GLM·offset · ⑧ 회귀 평가지표
