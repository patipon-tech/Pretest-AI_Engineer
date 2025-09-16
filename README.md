# Pretest-AI_Engineer
Pretest AI Engineer for candidate

# Generate Large Order Data CSV

โค้ดนี้ใช้เพิ่มขนาดไฟล์ CSV ของยอดขายสินค้า  
จาก ~1 ล้าน Record → 10 ล้าน Record

## 🛠️Requirements

- Python 3.8+
- pandas
- numpy

สามารถติดตั้ง dependencies ด้วย:

```bash
pip install -r requirements.txt
```

# Top 10 Spender Per Month

โปรเจกต์นี้ใช้สำหรับ **หา Top 10 spender ของแต่ละเดือน** จากไฟล์ยอดขายสินค้า

---

## 🛠️ Requirements

- Python 3.8+
- pandas

ติดตั้ง dependencies ด้วย:

```bash
pip install pandas
```

## Script ดึง Top 10 spender
```
import pandas as pd

# ----------------------------
# Script: top10_spender.py
# ----------------------------

# 1️⃣ โหลดไฟล์ CSV ขนาดใหญ่
df = pd.read_csv("large_order_data_10M.csv")

# 2️⃣ แปลงคอลัมน์วันที่เป็น datetime
df['transaction_datetime'] = pd.to_datetime(df['transaction_datetime'])

# 3️⃣ สร้างคอลัมน์ year_month
df['year_month'] = df['transaction_datetime'].dt.to_period('M')

# 4️⃣ รวมยอดขายต่อ customer ต่อเดือน
monthly_sales = df.groupby(['year_month', 'customer_no'], as_index=False)['amount'].sum()

# 5️⃣ หา Top 10 spender ของแต่ละเดือน
top10_each_month = (
    monthly_sales
    .sort_values(['year_month', 'amount'], ascending=[True, False])
    .groupby('year_month', group_keys=False)
    .head(10)
)

# 6️⃣ เพิ่มคอลัมน์ rank ต่อเดือน
top10_each_month['rank'] = top10_each_month.groupby('year_month')['amount'].rank(method='first', ascending=False)

# 7️⃣ บันทึกผลลัพธ์เป็น CSV
top10_each_month.to_csv("top10_spender_each_month.csv", index=False)

print("✅ Export CSV เรียบร้อย: top10_spender_each_month.csv")
```
