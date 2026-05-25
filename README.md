# CKD-Stages
## Veri Sızıntısından Arındırılmış Çok Sınıflı KBH Evrelemesi

> XGBoost + SMOTE + SHAP ile eGFR bağımlılığı olmaksızın Kronik Böbrek Hastalığı evre tahmini

---

### Proje Hakkında

Literatürdeki KBH makine öğrenmesi çalışmalarının büyük çoğunluğu, evreleri **tanımlayan** eGFR ve serum kreatinin değerlerini model girdisi olarak kullanır, bu durum **veri sızıntısı (data leakage)** oluşturur.

---

### Özellikler

- **Sızıntısız tasarım** — eGFR ve serum kreatinin özellik setinden tamamen çıkarıldı
- **KDIGO 2012 uyumlu** — Evre 1–5 arası 5 sınıflı etiketleme
- **SMOTE** ile sınıf dengesizliği giderimi (yalnızca eğitim setine uygulandı)
- **GridSearchCV** hiperparametre optimizasyonu
- **SHAP etkileşim analizi** — hipoalbüminemi × hipertansiyon sinerjik risk sinyali

---

### Veri Seti

| Özellik | Değer |
|---|---|
| Kaynak | UCI Machine Learning Repository |
| Kohort | n = 250 (yalnızca KBH pozitif hastalar) |
| Girdi değişkeni sayısı | 22 (eGFR ve SC çıkarıldıktan sonra) |
| Hedef | KBH Evresi (1–5) |

---

### Model Sonuçları

| Model | Ağırlıklı F1 | AUC-ROC | Sızıntısız |
|---|---|---|---|
| Temel Rastgele Orman | 0.51 | — | ✅ |
| XGBoost (GridSearchCV) | 0.58 | — | ✅ |
| **SMOTE + XGBoost (final)** | **0.59** | **0.808** | ✅ |
| Zheng ve ark. 2024 (kıyaslama) | — | 0.867 | ❌ |

> Performans farkı model yetersizliğini değil, gerçek tahmin zorluğunu yansıtır.

---

### SHAP Bulguları

- **En güçlü global özellik:** Kan üre azotu (bu)
- **Evre 5 için kritik:** Düşük hemoglobin (< 8 g/dL)
- **Sinerjik risk:** Hipoalbüminemi (< 3.5 g/dL) + Hipertansiyon (> 140 mmHg) → Evre 4–5 tahminine +0.04–0.06 SHAP birimi ek katkı

---

### Kullanılan Teknolojiler

```
Python · XGBoost · Scikit-learn · Imbalanced-learn (SMOTE)
SHAP · Pandas · NumPy · Matplotlib · Seaborn
```

---

### Proje Yapısı

```
├── data/
│   └── chronic_kidney_disease.arff   # UCI ham veri
├── notebooks/
│   └── kbh_evreleme.ipynb            # Ana analiz defteri
├── outputs/
│   ├── figures/                      # SHAP, kutu grafikleri vb.
│   └── KBH_Makale_Final.docx         # Akademik makale
└── README.md
```

---

### Referans

Çalışma KDIGO 2012 klinik rehberlerine dayanmaktadır:

> Kidney Disease: Improving Global Outcomes (KDIGO) CKD Work Group. *KDIGO 2012 Clinical Practice Guideline for the Evaluation and Management of Chronic Kidney Disease.* Kidney Int Suppl., 3, 1–150, 2013.
