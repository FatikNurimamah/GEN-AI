# Mini-GPT: Indonesian Text Generator

## 1. Deskripsi Proyek

Mini-GPT merupakan proyek Generative AI berupa model bahasa berbasis Decoder-Only Transformer yang dibangun dari awal menggunakan Python dan PyTorch.

Model dilatih menggunakan teks Bahasa Indonesia dari dataset Indonesian Wikipedia untuk mempelajari pola bahasa dan menghasilkan teks baru berdasarkan prompt yang diberikan pengguna.

Proyek ini mengimplementasikan mekanisme Transformer secara manual, termasuk token embedding, positional embedding, causal self-attention, multi-head attention, feed-forward network, residual connection, dan layer normalization.

## 2. Tujuan Proyek

1. Membangun model bahasa berbasis Decoder-Only Transformer menggunakan PyTorch.
2. Mengimplementasikan mekanisme self-attention dan causal masking secara manual.
3. Melatih model menggunakan dataset teks Bahasa Indonesia.
4. Menghasilkan teks baru berdasarkan prompt pengguna.
5. Mengevaluasi performa model menggunakan training loss, validation loss, dan perplexity.

## 3. Dataset

**Sumber:** Indonesian Wikipedia

https://huggingface.co/datasets/indonesian-nlp/wikipedia-id

Kolom utama yang digunakan adalah `text`, yaitu isi artikel Wikipedia berbahasa Indonesia.

Dataset akan melalui proses pembersihan sebelum digunakan untuk training. Ukuran dataset yang digunakan ditentukan berdasarkan kebutuhan eksperimen dan kemampuan perangkat.

## 4. Teknologi

- Python
- PyTorch
- Hugging Face Datasets
- NumPy
- Matplotlib
- tqdm

## 5. Arsitektur Model

Model menggunakan arsitektur Decoder-Only Transformer dengan komponen:

1. Token Embedding
2. Positional Embedding
3. Causal Self-Attention
4. Multi-Head Attention
5. Feed-Forward Network
6. Residual Connection
7. Layer Normalization
8. Linear Language Modeling Head

Model dilatih menggunakan Cross Entropy Loss dan optimizer AdamW untuk memprediksi token berikutnya.

**Catatan:** Implementasi attention dan Transformer dibuat secara manual. Penggunaan `nn.Transformer`, `nn.MultiheadAttention`, dan model pretrained tidak diperbolehkan.

## 6. Struktur Folder

```text
mini-gpt/
├── CONTEXT.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── dataset.py
│   ├── tokenizer.py
│   └── data_loader.py
├── model/
│   ├── attention.py
│   ├── transformer.py
│   └── model.py
├── train/
│   └── train.py
└── inference/
    ├── generate.py
    └── evaluate.py
```

## 7. Kontrak Fungsi Setiap Modul

### Folder data/

**dataset.py**
- Mengambil dataset dari Hugging Face.
- Membersihkan teks.
- Menggabungkan teks menjadi korpus.
- Menyimpan korpus ke penyimpanan lokal.

**tokenizer.py**
- Membuat vocabulary dari korpus.
- Mengubah teks menjadi token ID melalui `encode(text)`.
- Mengubah token ID menjadi teks melalui `decode(token_ids)`.
- Menyimpan dan memuat vocabulary.
- Menyediakan informasi `vocab_size`.

Metode tokenizer awal yang digunakan adalah character-level. Perubahan metode tokenizer harus disepakati sebelum implementasi.

**data_loader.py**
- Membaca korpus dan tokenizer.
- Membagi data menjadi training dan validation dengan rasio 90:10.
- Menghasilkan batch input dan target melalui `get_batch(split)`.
- Menghasilkan tensor `x` dan `y` dengan bentuk `(B, T)`.
- Target merupakan pergeseran satu token dari input.

### Folder model/

**attention.py**
- Mengimplementasikan Query, Key, dan Value.
- Menghitung scaled dot-product attention.
- Mengimplementasikan causal masking.
- Menggabungkan beberapa attention head.

**transformer.py**
- Mengimplementasikan Feed-Forward Network.
- Mengimplementasikan Transformer Block.
- Menggunakan LayerNorm dan residual connection.

**model.py**
- Mengimplementasikan class `MiniGPT`.
- Menggabungkan token embedding, positional embedding, Transformer Block, dan language modeling head.
- Menyediakan fungsi `forward(idx, targets=None)`.
- Menghasilkan logits dengan bentuk `(B, T, vocab_size)`.
- Menghitung Cross Entropy Loss ketika target tersedia.

### Folder train/

**train.py**
- Memuat data dan model.
- Menjalankan proses training.
- Menggunakan AdamW dan Cross Entropy Loss.
- Mengevaluasi training loss dan validation loss.
- Menyimpan checkpoint model terbaik.

### Folder inference/

**generate.py**
- Memuat checkpoint model.
- Menerima prompt dari pengguna.
- Menghasilkan token berikutnya secara autoregresif.
- Mendukung temperature dan top-k sampling.
- Mengubah token ID menjadi teks.

**evaluate.py**
- Memuat checkpoint model.
- Menghitung validation loss dan perplexity.
- Menampilkan grafik training loss dan validation loss.
- Menyajikan hasil evaluasi model.

## 8. Kontrak Antar-Modul

Alur data utama:

```text
dataset.py
    ↓
tokenizer.py
    ↓
data_loader.py
    ↓
model.py
    ↓
train.py
    ↓
checkpoint
    ↓
generate.py / evaluate.py
```

Ketentuan konsistensi:

- `vocab_size` harus berasal dari tokenizer.
- `block_size` harus konsisten antara data loader dan model.
- Input dan target menggunakan bentuk `(B, T)`.
- Logits model menggunakan bentuk `(B, T, vocab_size)`.
- Checkpoint harus menyimpan informasi konfigurasi yang dibutuhkan untuk memuat model kembali.
- Tokenizer yang digunakan saat inference harus sama dengan tokenizer saat training.

## 9. Aturan Pengembangan

1. Gunakan struktur folder yang telah ditentukan.
2. Gunakan nama fungsi dan variabel secara konsisten.
3. Setiap modul harus dapat diintegrasikan dengan modul lainnya.
4. Setiap perubahan kode harus diuji sebelum digabungkan ke branch utama.
5. Perubahan pada kontrak fungsi harus dikomunikasikan dan diperbarui di CONTEXT.md.
6. Hindari mengunggah dataset berukuran besar, checkpoint, file rahasia, dan virtual environment ke GitHub.
7. Gunakan Pull Request untuk menggabungkan perubahan ke branch `main`.

## 10. Target Akhir

Proyek dinyatakan berhasil apabila:

- Dataset berhasil diproses.
- Tokenizer berhasil melakukan encoding dan decoding.
- Model Mini-GPT berhasil dibangun.
- Training dapat dijalankan.
- Checkpoint berhasil disimpan dan dimuat kembali.
- Evaluasi model berhasil dilakukan.
- Model dapat menghasilkan teks Bahasa Indonesia berdasarkan prompt pengguna.
