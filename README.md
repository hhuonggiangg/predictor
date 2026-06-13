# 🎵 Spotify Popularity Predictor

Streamlit app dự đoán độ phổ biến của bài hát dựa trên audio features, kèm
giao diện động (floating notes, aurora, gauge & radar tương tác, confetti…).

---

## ⚠️ Đọc trước khi deploy — về Vercel

**Vercel KHÔNG chạy được Streamlit một cách ổn định.** Vercel dành cho serverless
functions / static site, trong khi Streamlit cần một server chạy liên tục với
kết nối WebSocket. Các cấu hình `vercel.json` + `api/index.py` trong repo này chỉ
là **best-effort** — có thể timeout hoặc không giữ được kết nối.

👉 **Nền tảng nên dùng (đều miễn phí, chạy ổn định):**

| Nền tảng | Độ dễ | Ghi chú |
|---|---|---|
| **Streamlit Community Cloud** | ⭐ Dễ nhất | Sinh ra cho Streamlit, chỉ cần repo + `requirements.txt` |
| **Render** | Dễ | Đã có sẵn `render.yaml` |
| **Hugging Face Spaces** | Dễ | Chọn SDK = Streamlit |
| **Railway / Fly.io** | Trung bình | Dùng `Procfile` hoặc `Dockerfile` |
| **Docker (bất kỳ host nào)** | Trung bình | Đã có sẵn `Dockerfile` |

---

## 📁 Cấu trúc thư mục

```
spotify-predictor/
├── app.py                  # App chính
├── requirements.txt
├── Procfile                # Render / Railway / Heroku-style
├── render.yaml             # Render (1-click)
├── Dockerfile              # Bất kỳ container host nào
├── vercel.json             # Vercel (best-effort)
├── api/
│   └── index.py            # Vercel entry (best-effort)
├── .streamlit/
│   └── config.toml         # Theme + server config
├── data/                   # (tùy chọn) đặt spotify_songs_expanded.csv ở đây
│   └── spotify_songs_expanded.csv
└── model_lr.pkl            # (tùy chọn) model đã train
```

> **Lưu ý:** Nếu **không** có `data/spotify_songs_expanded.csv` và `model_lr.pkl`,
> app vẫn chạy bình thường: nó tự sinh dataset mẫu và dùng công thức dự đoán
> dự phòng. Muốn dùng dữ liệu/model thật thì copy 2 file đó vào đúng vị trí trên.

---

## 🚀 Cách 1 — Streamlit Community Cloud (khuyến nghị, dễ nhất)

1. Push code lên một repo GitHub (public hoặc private).
2. Vào https://share.streamlit.io → **Create app** → chọn repo.
3. Main file path: `app.py` → **Deploy**.

Xong. Mỗi lần push lên GitHub app tự deploy lại.

---

## 🚀 Cách 2 — Render

1. Push code lên GitHub.
2. Vào https://render.com → **New** → **Blueprint** → chọn repo (Render tự đọc `render.yaml`).
   - Hoặc **New Web Service** thủ công với:
     - Build: `pip install -r requirements.txt`
     - Start: `streamlit run app.py --server.port $PORT --server.address 0.0.0.0 --server.headless true`
3. **Create** → đợi build xong là có link.

---

## 🚀 Cách 3 — Hugging Face Spaces

1. Tạo Space mới, **SDK = Streamlit**.
2. Upload `app.py` + `requirements.txt` (+ `data/`, `model_lr.pkl` nếu có).
3. Space tự build và chạy.

---

## 🚀 Cách 4 — Docker (Railway, Fly.io, VPS…)

```bash
docker build -t spotify-predictor .
docker run -p 8501:8501 spotify-predictor
# mở http://localhost:8501
```

---

## 🟡 Cách 5 — Vercel (best-effort, KHÔNG khuyến nghị)

```bash
npm i -g vercel
vercel
```

Repo đã có `vercel.json` và `api/index.py`. Nếu nó không hoạt động (rất có thể),
hãy dùng một trong các cách trên. Đây là giới hạn kiến trúc của Vercel, không
phải lỗi code.

---

## 💻 Chạy local

```bash
pip install -r requirements.txt
streamlit run app.py
# http://localhost:8501  —  đăng nhập demo: demo / demo123
```

---

## ✨ Tính năng giao diện đã nâng cấp

- Nền động: floating music notes + aurora glow blobs
- Gauge tương tác (Plotly) cho điểm dự đoán
- Radar "Audio DNA" cập nhật **trực tiếp** khi kéo slider
- Hiệu ứng: neon gradient title, ripple trên nút, glassmorphism, celebration pulse
- Toast notifications + balloons khi predict / lưu favorite
- Custom cursor, equalizer động, hover micro-interactions
