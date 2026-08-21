# Bao cao Lab MLOps - CI/CD cho AI Systems

**Ho ten:** Ly Minh Hai
**MSSV:** 2A202601503
**Ngay:** 21/08/2026

---

## 1. Bo sieu tham so da chon va ly do

**Bo tham so tot nhat:** `n_estimators=500, max_depth=None, min_samples_split=2`

Da thu nghiem 5 bo tham so khac nhau tren MLflow:

| n_estimators | max_depth | min_samples_split | Accuracy |
|---|---|---|---|
| 100 | 5 | 2 | 0.5640 |
| 50 | 3 | 2 | 0.5580 |
| 200 | 10 | 5 | 0.6440 |
| 300 | 15 | 10 | 0.6700 |
| **500** | **None** | **2** | **0.7460** |

**Ly do chon:** RandomForest voi so luong cay lon (500) va depth khong gioi han cho phep mo hinh hoc duoc cac pattern phuc tap trong du lieu. Du lieu wine quality co nhieu feature lien quan den chat luong ruou nen mo hinh can du sau de bat cac quan he phi tuyen.

---

## 2. Bang so sanh ket qua

| Chi so | Buoc 2 (2998 mau) | Buoc 3 (5996 mau) | Thay doi |
|---|---|---|---|
| **accuracy** | 0.6700 | 0.7460 | **+7.6%** |
| **f1_score** | 0.6682 | 0.7451 | **+7.7%** |

**Nhan xet:** Khi so luong mau tang tu 2998 len 5996 (gấp doi), do chinh xac cua mo hinh tang dang ke tu 67% len 74.6%. Dieu nay chung minh rang them du lieu huấn luyen giup mo hinh hoc duoc cac pattern tot hon.

---

## 3. Kho gap va cach giai quyet

| Kho gap | Cach giai quyet |
|---|---|
| Accuracy < 0.70 voi 2998 mau | Them du lieu moi (train_phase2.csv) tang len 5996 mau |
| SSH key format sai trong GitHub Actions | Tao lai SSH key va cap nhat secret VM_SSH_KEY |
| DVC khong tim thay credentials | Cau hinh DVC remote truc tiep trong workflow |
| Service chua khoi dong kip khi health check | Tang sleep tu 5s len 10s + retry 3 lan |

---

## 4. Ket qua cuoi cung

- Pipeline CI/CD hoat dong hoan toan tu dong
- Khi commit du lieu moi, pipeline tu dong: Test -> Train -> Eval -> Deploy
- Model moi (0.746 accuracy) da duoc trien khai len VM
- Endpoint `/predict` hoat dung chinh xac

---

## 5. Proof of completion

- MLflow UI: 5 experiments da ghi nhan (xem screenshot b1.png)
- GitHub Actions: 4 jobs (Test, Train, Eval, Deploy) thanh cong (xem screenshot b2.png)
- Service endpoints:
  - `GET /health` -> `{"status": "ok"}`
  - `POST /predict` -> `{"prediction": 0, "label": "thap"}`
- Cloud Storage: du lieu va model da duoc push len S3
