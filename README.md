# Auction Ranking POC – README

## 1. วัตถุประสงค์
พัฒนา Proof of Concept (POC) เพื่อทดสอบการใช้ AI และเครื่องมือที่เหมาะสมสำหรับ
การ **คัดเลือกและจัดเรียงสินค้าประมูล** ให้ตรงตามความต้องการของลูกค้า
โดยเน้นความแม่นยำ ความคุ้มค่า และความเร็วในการประมวลผล

---

## 2. ขอบเขต POC
- ใช้ข้อมูลสินค้า, ความต้องการลูกค้า, และพฤติกรรมการใช้งานเพื่อจัดลำดับสินค้า
- ทดลองทั้ง **Rule-based**, **Machine Learning Ranking (LTR)** และ **Hybrid Approach**
- ใช้ AWS เป็นหลัก พร้อมสำรวจตัวเลือก Low-cost
- ประเมินด้วย Offline metrics และ Mock UI

---

## 3. Process & Manday

| Phase | Activities | Deliverables | Owner | Manday |
|-------|------------|--------------|-------|--------|
| 1. Data Understanding | สำรวจข้อมูลสินค้า + ความต้องการลูกค้า | Data inventory | BA/Data | 1 |
| 2. Data Cleaning | เตรียมและ clean dataset | Clean dataset | Data Eng | 2 |
| 3. Feature Engineering | ฟีเจอร์ price match, brand/attr match, urgency, popularity, seller score | Feature dataset | DS | 2 |
| 4. Baseline (Rule-based) | Weighted score ranking | Baseline demo | DS | 1 |
| 5. ML Ranking (LTR) | Train LightGBM/XGBoost LambdaRank | Model + report | DS | 3 |
| 6. Vector Similarity (Optional) | Embedding ข้อความ + similarity score | Vector demo | DS | 1 |
| 7. Evaluation | Offline metrics + UI mock | Evaluation report | DS/BA | 1 |
| 8. Report | สรุปผลและข้อเสนอแนะ | Slides | PM/DS | 1 |
| **รวม** |  |  |  | **12** |

---

## 4. Tools & Cost

| Category | AWS Service | Purpose | POC Cost (est.) | Low-cost Alternative |
|----------|-------------|---------|-----------------|----------------------|
| Storage | Amazon S3 | เก็บข้อมูล/ผลโมเดล | $1–3/เดือน | MinIO, Google Drive |
| Data Prep | AWS Glue / DataBrew | Clean/Transform | $3–5 | Pandas local |
| Modeling | SageMaker Notebook | Train LTR model | $1–5 | Colab, Local |
| Search/Vector | Amazon OpenSearch | Vector similarity | $18/เดือน | Elasticsearch local, FAISS |
| Serving | Lambda + API Gateway | Mock API | ฟรี | FastAPI local |
| BI | QuickSight | วิเคราะห์ผล | $12/ผู้ใช้ | Excel, Looker Studio |

---

## 5. Prediction & AI Approach

1. **Baseline Weighted Score** – Rule-based ranking  
2. **Learning to Rank (LTR)** – LightGBM/XGBoost LambdaRank  
3. **Vector Similarity** – Sentence Transformers + kNN search  
4. **Hybrid Ranking** – รวมคะแนนจาก LTR + Vector + Rule-based

---

## 6. Metrics

| Metric | Definition | Target | Remark |
|--------|------------|--------|--------|
| Precision@K | สัดส่วนสินค้าที่ตรงใน Top-K | >= 0.6 | ยิ่งสูงยิ่งดี |
| NDCG@K | คุณภาพการจัดอันดับ | >= 0.7 | คำนึงตำแหน่ง |
| CTR uplift | คลิกเพิ่มขึ้นจาก baseline | +5–15% | A/B test |
| Add-to-bid | อัตราเพิ่มลงประมูล | +3–10% | A/B test |

---

## 7. Risks & Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Data sparsity | ข้อมูลพฤติกรรมไม่พอ | เริ่มจาก rule-based + expert labels |
| Cold-start products | จัดอันดับสินค้ามาใหม่ยาก | ใช้ content-based + vector similarity |
| Cost constraints | งบ AWS จำกัด | ใช้ local stack ระหว่าง POC |

---

## 8. การใช้งานไฟล์ Excel POC
1. เปิดไฟล์ `Auction_Ranking_POC_Manday.xlsx`
2. แก้ไขค่าต่าง ๆ ในชีท **Config** ให้ตรงกับเงื่อนไขลูกค้า
3. ปรับน้ำหนัก (Weights) เพื่อทดลองจัดลำดับ
4. ดูผลลัพธ์ในชีท **Ranking** และเรียงตาม **TotalScore**
