# `class2.ipynb` 명령어 요약

이 문서는 노트북에서 사용한 Python·pandas 명령어를 기능별로 모아 설명합니다. 아래 예시는 노트북의 데이터프레임 이름(`df`, `df2`, `df_scores` 등)을 그대로 사용합니다.

## 1. 라이브러리 불러오기와 엑셀 읽기

```python
!pip install openpyxl
import pandas as pd

df = pd.read_excel("모의데이터.xlsx")
df_scores = pd.read_excel("모의데이터.xlsx", sheet_name="성적")
df_attendance = pd.read_excel("학생정보.xlsx", sheet_name="출석정보")
df_clubs = pd.read_excel("학생정보.xlsx", sheet_name="동아리정보")
df_meals = pd.read_excel("학생정보.xlsx", sheet_name="급식선호도")
```

- `!pip install openpyxl`: 노트북에서 엑셀 파일을 읽는 데 필요한 패키지를 설치합니다. 이미 설치했다면 다시 실행할 필요는 없습니다.
- `import pandas as pd`: pandas를 `pd`라는 짧은 이름으로 불러옵니다.
- `pd.read_excel(...)`: 엑셀 파일을 데이터프레임으로 읽습니다. `sheet_name`으로 읽을 시트를 지정합니다.
- `engine="openpyxl"`은 엑셀 읽기 엔진을 직접 지정할 때 사용합니다.

## 2. 직접 데이터 만들기와 확인하기

```python
data = [
    {"학년": 1, "반": 1, "번호": 1, "이름": "가상 이름", "성별": "여",
     "수학": 79.0, "영어": 73.0, "과학": 81.0},
]
df4 = pd.DataFrame(data)
df
```

- `data`: 딕셔너리 여러 개를 담은 목록입니다. 각 딕셔너리가 한 행이고, 항목 이름이 열 이름입니다.
- `pd.DataFrame(data)`: 목록을 데이터프레임으로 바꿉니다.
- 셀 마지막에 `df`처럼 데이터프레임 이름을 적으면 노트북에 표를 표시합니다.

## 3. 데이터프레임의 구조와 행·열 확인

```python
df.columns
df.index
df.info()
df.tail()
df
```

- `df.columns`: 열 이름을 확인합니다. `df.columns[0]`처럼 쓰면 특정 위치의 열 이름을 가져옵니다.
- `df.index`: 행의 인덱스(행 번호)를 확인합니다.
- `df.info()`: 행·열 개수, 각 열의 자료형, 비어 있지 않은 값의 개수 등을 요약합니다.
- `df.tail()`: 마지막 5개 행을 확인합니다. `df.tail`처럼 괄호를 빼면 실행이 아니라 메서드 자체를 가리킵니다.
- `df`: 표 전체를 표시합니다.

## 4. 열과 행 선택하기

```python
df["과학"]
df[["이름", "수학", "영어"]]
df[0:20]
df.loc[0:1, ["이름", "성별"]]
df.loc[df["성별"] == "여", ["이름", "영어"]]
df.loc[df["성별"] == "남", ["이름", "과학"]]
df.loc[df["반"] == 3, ["영어", "과학"]]
```

- `df["과학"]`: 한 열을 선택합니다.
- `df[["이름", "수학", "영어"]]`: 여러 열을 선택합니다. 열 이름 목록은 대괄호 두 겹으로 씁니다.
- `df[0:20]`: 앞에서부터 20개 행을 선택합니다.
- `df.loc[행 조건, 열 목록]`: 조건에 맞는 행에서 지정한 열을 선택합니다.
- `==`는 같은지 비교하고, `>`는 초과인지 비교합니다.

## 5. 조건에 맞는 행과 결측치 다루기

```python
df5 = df[df["과학"] > 70]
df.isnull()
df.isnull().sum()
df.fillna(0)
df.dropna(subset=["과학"])
df2 = df.dropna()
```

- `df[df["과학"] > 70]`: 과학 점수가 70점보다 큰 행만 선택합니다.
- `df.isnull()`: 비어 있는 값인지 각 칸별로 `True`/`False`를 표시합니다.
- `df.isnull().sum()`: 열마다 결측치 개수를 셉니다.
- `df.fillna(0)`: 결측치를 0으로 바꾼 결과를 만듭니다.
- `df.dropna(subset=["과학"])`: 과학 값이 비어 있는 행을 제외합니다.
- `df.dropna()`: 하나 이상의 열에 결측치가 있는 행을 제외합니다.

`fillna`와 `dropna`의 결과를 계속 사용하려면 변수에 저장해야 합니다. 예: `df = df.fillna(0)`.

## 6. 새 열 계산하기와 반올림

```python
df2["총점"] = df2["수학"] + df2["영어"] + df2["과학"]
df2["평균"] = df2["총점"] / 3
df2["평균"] = df2["평균"].round(1)
df2["평균2"] = df2[["수학", "영어", "과학"]].mean(axis="columns").round(2)
```

- `df2["새 열"] = ...`: 계산 결과를 새 열로 추가하거나 기존 열을 갱신합니다.
- `+`: 행별 점수를 더해 총점을 계산합니다.
- `.mean(axis="columns")`: 각 행의 지정한 과목 점수 평균을 계산합니다. `axis=1`도 같은 의미입니다.
- `.round(1)` / `.round(2)`: 소수점 첫째 자리 / 둘째 자리까지 반올림합니다.

## 7. 열 삭제하기

```python
df2 = df2.drop(columns=["평균2"])
```

- `drop(columns=[...])`: 지정한 열을 제거한 새 데이터프레임을 반환합니다.
- 반환값을 `df2`에 다시 저장해야 이후에도 삭제 상태가 유지됩니다.
- 노트북의 `inplace=True` 방식은 pandas에서 사용 중단 예정 경고가 나올 수 있으므로 위처럼 재할당하는 방법을 권장합니다.

## 8. 그룹별 평균 구하기

```python
df2.groupby("반")[["수학", "영어", "과학"]].mean().round(2)
df2.groupby("성별")[["수학", "영어", "과학"]].mean().round(2)

mean_subject_class = df4.groupby("반")[["수학", "영어", "과학"]].mean().round(1)
mean_class = df4.groupby("반")[["평균"]].mean().round(1)
mean_gender = df4.groupby("성별")[["수학", "영어", "과학"]].mean().round(1)
mean_club = df4.groupby("동아리명")[["평균"]].mean().round(1)
```

- `groupby("열")`: 해당 열의 값별로 데이터를 묶습니다. 예를 들어 `"반"`이면 반별로 묶습니다.
- `[["수학", "영어", "과학"]]`: 평균을 계산할 열을 고릅니다.
- `.mean()`: 각 그룹의 평균을 계산합니다.
- `.round(1)`: 결과를 소수점 첫째 자리까지 반올림합니다.
- `mean_club`은 동아리별 평균을 담는 변수입니다. 같은 변수에 다시 저장하면 이전 결과가 새 결과로 바뀝니다.

## 9. 데이터프레임 합치기

```python
df1 = df_scores.merge(df_attendance, on="이름", how="inner")

df_scores["학생ID"] = (
    df_scores["학년"] * 1000
    + df_scores["반"] * 100
    + df_scores["번호"]
)
df1 = df_scores.merge(df_attendance, on="학생ID", how="inner")
df2 = df1.merge(df_clubs, on="학생ID", how="left")
df3 = df2.merge(df_meals, on="학생ID", how="left")
df4 = df3.drop(columns=["이름_y"])
```

- `.merge(...)`: 공통된 기준 열을 사용해 두 데이터프레임을 합칩니다.
- `on="학생ID"`: 학생ID가 같은 행끼리 연결합니다. `on="이름"`은 이름으로 연결하는 예입니다.
- `how="inner"`: 양쪽에 모두 존재하는 기준 값만 남깁니다.
- `how="left"`: 왼쪽 데이터프레임의 행을 모두 유지하고, 오른쪽에서 일치하는 정보를 붙입니다. 일치하지 않으면 결측치가 됩니다.
- `학생ID`: 학년·반·번호를 조합해 학생별 식별값을 만듭니다.
- 같은 이름의 열이 양쪽에 있으면 pandas가 `_x`, `_y`를 붙여 구분할 수 있습니다.
- 노트북의 `dk = df1`은 `df1`을 다른 이름으로도 가리키게 합니다.

## 10. 합친 데이터 정리, 석차와 정렬

```python
df4[["수학", "과학", "영어"]] = df4[["수학", "과학", "영어"]].fillna(0)
df4["평균"] = df4[["수학", "과학", "영어"]].mean(axis=1).round(1)
df4["반내석차"] = df4.groupby("반")["평균"].rank(
    ascending=False, method="min"
)
df4["전교석차"] = df4["평균"].rank(ascending=False, method="min")

df_top10 = df4.sort_values(by="평균", ascending=False).head(10)
df_bottom10 = df4.sort_values(by="평균", ascending=True).head(10)
```

- `fillna(0)`: 세 과목의 결측치를 0점으로 바꿉니다. 오른쪽의 계산 결과를 왼쪽 열들에 저장합니다.
- `.rank(ascending=False)`: 큰 평균을 높은 순위로 매깁니다.
- `method="min"`: 동점자는 같은 순위로 처리하고, 다음 순위는 건너뜁니다.
- `.sort_values(by="평균", ascending=False)`: 평균 내림차순으로 정렬합니다.
- `.head(10)`: 정렬된 결과의 처음 10개 행을 가져옵니다. 오름차순으로 정렬하면 평균이 낮은 10개가 됩니다.

## 확인하면 좋은 부분

- `df.info()`의 `Non-Null Count`는 **비어 있지 않은 값의 개수**입니다. 결측치 개수를 직접 확인하려면 `df.isnull().sum()`을 사용합니다.
- `df.fillna(0)`처럼 결과를 변수에 저장하지 않으면 원래 데이터프레임은 바뀌지 않습니다.
- `df.tail`은 괄호가 빠져 있으므로 마지막 행을 보려면 `df.tail()`로 씁니다.
- 반별/학년별 평균을 계산할 때는 조건식에 원본 `df`가 아닌 실제 계산 대상인 `df2`를 사용하는 것이 안전합니다. 예: `df2.loc[df2["반"] == 1]`.
- 노트북의 반별 계산 부분에서 변수명이 `df_calss1`로 오타가 있고, 학년과 반 조건을 연속으로 대입하는 셀은 첫 번째 조건 결과를 덮어씁니다. 학년과 반을 동시에 고르려면 아래처럼 조건을 함께 적용합니다.

```python
df_class1 = df2.loc[(df2["학년"] == 1) & (df2["반"] == 1)]
```
