# LAB 04 — Pandas Fundamentals: จาก Spreadsheet สู่ Reproducible Analysis
**Day 2 | ANALYZE & INTERPRET**  
**เวลา:** 55 นาที | **รูปแบบ:** Individual | **เครื่องมือ:** GitHub Codespaces, VS Code, Python/Pandas

## เป้าหมายของ Lab
ให้ผู้เรียนสามารถเปิด Dataset, ตรวจโครงสร้าง และสรุปข้อมูลด้วย Pandas โดยเข้าใจว่าแต่ละคำสั่งช่วยตอบ Business Question อย่างไร

> เป้าหมายไม่ใช่ “จำ Syntax” แต่คือสามารถอ่าน/ปรับ Code เพื่อวิเคราะห์ข้อมูลของตนเองได้

---

## ไฟล์ที่ใช้

```text
day02/case02_marketing/data/marketing_performance.csv
day02/starter/analysis_starter.py
```

---

# ก่อนเริ่ม — ตรวจ Workspace (5 นาที)
1. เปิด Repository ใน Codespaces
2. ดู Explorer ด้านซ้ายว่ามี `day02/`
3. เปิด Terminal
4. ตรวจ current folder:

```bash
pwd
```

5. ตรวจ Python/Pandas:

```bash
python --version
python -c "import pandas as pd; print(pd.__version__)"
```

ถ้าไม่ผ่าน ให้หยุดและแจ้งผู้สอน

---

# ขั้นตอนการทดลอง

## Step 1 — เปิด Starter Script และ Run ครั้งแรก (5 นาที)
เปิด:

```text
day02/starter/analysis_starter.py
```

Run:

```bash
python day02/starter/analysis_starter.py
```

### ควรเห็น
- 5 แถวแรก
- รายชื่อ columns
- data type
- จำนวน rows ที่ไม่เป็น null

### ถ้า FileNotFoundError
ตรวจว่า run จาก root ของ repository หรือไม่

---

## Step 2 — อ่านข้อมูลด้วย `read_csv()` และดูตัวอย่าง (5 นาที)
Code:

```python
import pandas as pd

df = pd.read_csv("day02/case02_marketing/data/marketing_performance.csv")
print(df.head())
```

เขียนคำตอบ:

```text
หนึ่งแถวของ Dataset นี้แทนอะไร?
____________________________________
```

> การรู้ “grain” หรือความหมายของ 1 row สำคัญมากก่อน groupby

---

## Step 3 — ตรวจโครงสร้างด้วย `info()` (5 นาที)

```python
df.info()
```

สังเกต:
- จำนวน rows/columns
- data type
- null count โดยคร่าว

ตอบ:

```text
Column ใดเป็น Dimension?
Column ใดเป็น Metric?
Column ใดควรเป็น Date?
```

---

## Step 4 — สรุป Numeric ด้วย `describe()` (5 นาที)

```python
print(df.describe())
```

อย่าอ่านทุกตัวเลข ให้หาเพียง:
- ค่าที่ดูสูง/ต่ำผิดปกติ
- ช่วงของ Spend/Revenue/ROAS
- สิ่งที่ควรตรวจเพิ่ม  ค่าที่ออกมา คือ
              Spend   Impressions  ...  Conversion_Rate         ROAS
count    320.000000  3.200000e+02  ...       320.000000   320.000000
mean    8324.066031  5.447139e+05  ...         0.050334   166.470011
std     4223.786908  3.145730e+05  ...         0.025372   233.129407
min     1521.840000  7.469300e+04  ...         0.013170     3.438378
25%     4592.990000  2.811028e+05  ...         0.029572    39.594240
50%     7961.005000  5.066650e+05  ...         0.048226   100.747528
75%    11732.460000  7.584050e+05  ...         0.066999   193.009121
max    15993.890000  1.377480e+06  ...         0.147899  2510.999786

[8 rows x 8 columns]
- 

### Checkpoint
`describe()` ไม่ตอบ Business Question โดยตรง แต่ช่วย “รู้จักข้อมูล” ก่อนวิเคราะห์

---

## Step 5 — Data Quality Check แบบสั้น (5 นาที)

```python
print(df.isnull().sum())
print("duplicates:", df.duplicated().sum())
```
สิ่งที่ได้ 
[8 rows x 8 columns]
Month               0
Campaign            0
Channel             0
Customer_Segment    0
Spend               0
Impressions         0
Clicks              0
Conversions         0
Revenue             0
CTR                 0
Conversion_Rate     0
ROAS                0
dtype: int64
duplicates: 0

ตรวจ category:

```python
print(df["Channel"].value_counts())
print(df["Customer_Segment"].value_counts())
```
สิ่งที่ได้ 
    Month Campaign    Channel  ...       CTR  Conversion_Rate        ROAS
0  2026-04-01   CMP-15     Social  ...  0.033970         0.037371  210.962836
1  2026-02-01   CMP-14  Affiliate  ...  0.025994         0.053035  107.251179
2  2026-02-01   CMP-03      Email  ...  0.068372         0.062505  170.733945
3  2026-07-01   CMP-09  Affiliate  ...  0.042127         0.058195  168.499280
4  2026-01-01   CMP-11     Search  ...  0.050429         0.060678  125.788451

[5 rows x 12 columns]
<class 'pandas.DataFrame'>
RangeIndex: 320 entries, 0 to 319
Data columns (total 12 columns):
 #   Column            Non-Null Count  Dtype  
---  ------            --------------  -----  
 0   Month             320 non-null    str    
 1   Campaign          320 non-null    str    
 2   Channel           320 non-null    str    
 3   Customer_Segment  320 non-null    str    
 4   Spend             320 non-null    float64
 5   Impressions       320 non-null    int64  
 6   Clicks            320 non-null    int64  
 7   Conversions       320 non-null    int64  
 8   Revenue           320 non-null    float64
 9   CTR               320 non-null    float64
 10  Conversion_Rate   320 non-null    float64
 11  ROAS              320 non-null    float64
dtypes: float64(5), int64(3), str(4)
memory usage: 39.1 KB
None
              Spend   Impressions  ...  Conversion_Rate         ROAS
count    320.000000  3.200000e+02  ...       320.000000   320.000000
mean    8324.066031  5.447139e+05  ...         0.050334   166.470011
std     4223.786908  3.145730e+05  ...         0.025372   233.129407
min     1521.840000  7.469300e+04  ...         0.013170     3.438378
25%     4592.990000  2.811028e+05  ...         0.029572    39.594240
50%     7961.005000  5.066650e+05  ...         0.048226   100.747528
75%    11732.460000  7.584050e+05  ...         0.066999   193.009121
max    15993.890000  1.377480e+06  ...         0.147899  2510.999786

[8 rows x 8 columns]
Month               0
Campaign            0
Channel             0
Customer_Segment    0
Spend               0
Impressions         0
Clicks              0
Conversions         0
Revenue             0
CTR                 0
Conversion_Rate     0
ROAS                0
dtype: int64
duplicates: 0
Channel
Display      75
Social       67
Affiliate    67
Search       56
Email        55
Name: count, dtype: int64
Customer_Segment
New          155
Returning    126
HighValue     39
Name: count, dtype: int64

### Reflection
สิ่งที่ Google Sheets ทำด้วย Filter/Pivot เมื่อวาน วันนี้ Python ทำซ้ำได้ด้วย Code

---

## Step 6 — Filter ด้วย `query()` (5 นาที)
ตัวอย่าง:

```python
email = df.query("Channel == 'Email'")
print(email.head())
```

ลอง Filter:

```python
high_roas = df.query("ROAS > 3")
```
Channel
Display      75
Social       67
Affiliate    67
Search       56
Email        55
Name: count, dtype: int64
Customer_Segment
New          155
Returning    126
HighValue     39
Name: count, dtype: int64
ตอบ:

> Filter นี้ช่วยตอบคำถามอะไร?

---

## Step 7 — Groupby ครั้งแรก (8 นาที)

```python
channel_summary = (
    df.groupby("Channel", as_index=False)
      .agg(
          Spend=("Spend", "sum"),
          Revenue=("Revenue", "sum"),
          Conversions=("Conversions", "sum")
      )
)

print(channel_summary)
```

0  2026-04-01   CMP-15     Social  ...  0.033970         0.037371  210.962836
1  2026-02-01   CMP-14  Affiliate  ...  0.025994         0.053035  107.251179
2  2026-02-01   CMP-03      Email  ...  0.068372         0.062505  170.733945
3  2026-07-01   CMP-09  Affiliate  ...  0.042127         0.058195  168.499280
4  2026-01-01   CMP-11     Search  ...  0.050429         0.060678  125.788451

              Spend   Impressions  ...  Conversion_Rate         ROAS
count    320.000000  3.200000e+02  ...       320.000000   320.000000
mean    8324.066031  5.447139e+05  ...         0.050334   166.470011
std     4223.786908  3.145730e+05  ...         0.025372   233.129407
min     1521.840000  7.469300e+04  ...         0.013170     3.438378
25%     4592.990000  2.811028e+05  ...         0.029572    39.594240
50%     7961.005000  5.066650e+05  ...         0.048226   100.747528
75%    11732.460000  7.584050e+05  ...         0.066999   193.009121
max    15993.890000  1.377480e+06  ...         0.147899  2510.999786

### อ่าน Code ทีละส่วน
- `groupby("Channel")` → แบ่งข้อมูลตาม Channel
- `.agg(...)` → สรุป Metric ของแต่ละกลุ่ม
- `as_index=False` → ให้ Channel ยังเป็น column ปกติ

### Business Purpose
> เปรียบเทียบ scale ของ Spend/Revenue/Conversions ระหว่าง Channel

---

## Step 8 — Sort เพื่อ Rank (5 นาที)

```python
print(channel_summary.sort_values("Revenue", ascending=False))
```

ลองเปลี่ยนเป็น Sort ด้วย Spend หรือ metric อื่น

### คำถาม
Ranking เปลี่ยนไหมเมื่อเปลี่ยน Metric?

นี่คือสัญญาณว่าไม่ควรตัดสินใจจาก metric เดียว

---

## Step 9 — Save Output (4 นาที)
สร้าง folder ถ้ายังไม่มี:

```bash
mkdir -p day02/output
```

ใน Python:

```python
channel_summary.to_csv("day02/output/channel_summary.csv", index=False)
```

ตรวจใน Explorer ว่าไฟล์ถูกสร้าง

---

# Stop & Explain
ผู้สอนอาจสุ่มถาม:

> `groupby()` block นี้ Input คืออะไร → ทำอะไร → Output คืออะไร → ช่วย Decision อย่างไร?

ห้ามตอบเพียง “Copilot เขียนให้”

---

# สิ่งที่ต้องส่ง
- Script ที่ run ได้
- `channel_summary.csv`
- คำอธิบาย Code 1 block ด้วยภาษาของตนเอง
- Business Purpose ของคำสั่งสำคัญ ≥3 จุด

---

# Core / Challenge

## Core
ทำ Step 1–9 ให้ครบ

## Challenge
สร้าง function:

```python
def summarize_by(df, dimension):
    # return Spend / Revenue / Conversions summary
    ...
```

แล้วทดลองกับ `Channel` และ `Customer_Segment`

---

# Common Mistakes
- Run script จาก folder ผิดจนหา CSV ไม่เจอ
- ใช้ `mean()` กับ Metric ที่ควร sum โดยไม่คิด Business Meaning
- Groupby ก่อนรู้ว่า 1 row หมายถึงอะไร
- Copy code จาก AI แล้วอธิบายไม่ได้

**จำไว้:** Reproducible Analytics = คนอื่น Run code เดิมแล้วได้วิธีวิเคราะห์เดียวกัน
