# Apache Kafka - Big Data

| Nama Lengkap         | NRP        |
| -------------------- | ---------- |
| Maulana Ahmad Zahiri | 5027231010 |

## Soal

`Problem Based Learning : Apache Kafka`

🎯 Latar Belakang Masalah
Sebuah perusahaan logistik mengelola beberapa gudang penyimpanan yang menyimpan barang sensitif seperti makanan, obat-obatan, dan elektronik. Untuk menjaga kualitas penyimpanan, gudang-gudang tersebut dilengkapi dengan dua jenis sensor:

- Sensor Suhu
- Sensor Kelembaban

Sensor akan mengirimkan data setiap detik. Perusahaan ingin memantau kondisi gudang secara real-time untuk mencegah kerusakan barang akibat suhu terlalu tinggi atau kelembaban berlebih.

---

`📋 Tugas Mahasiswa`

`1. Buat Topik Kafka`

Buat dua topik di Apache Kafka:

- sensor-suhu-gudang
- sensor-kelembaban-gudang

Topik ini akan digunakan untuk menerima data dari masing-masing sensor secara real-time.

---

`2. Simulasikan Data Sensor (Producer Kafka)`

Buat dua Kafka producer terpisah:

- Producer Suhu
  Kirim data setiap detik

  Format: `{"gudang_id": "G1", "suhu": 82}`

- Producer Kelembaban
  Kirim data setiap detik

  Format: `{"gudang_id": "G1", "kelembaban": 75}`

  Gunakan minimal 3 gudang: G1, G2, G3.

---

`3. Konsumsi dan Olah Data dengan PySpark`

**a. Buat PySpark Consumer**

Konsumsi data dari kedua topik Kafka.

**b. Lakukan Filtering:**

- Suhu > 80°C → tampilkan sebagai peringatan suhu tinggi
- Kelembaban > 70% → tampilkan sebagai peringatan kelembaban tinggi

Contoh Output:

```bash
[Peringatan Suhu Tinggi]
Gudang G2: Suhu 85°C
```

```bash
[Peringatan Kelembaban Tinggi]
Gudang G3: Kelembaban 74%
```

**c. Buat Peringatan Gabungan:**

Jika ditemukan suhu > 80°C dan kelembaban > 70% pada gudang yang sama, tampilkan peringatan kritis.

✅ Contoh Output Gabungan:

[PERINGATAN KRITIS]
Gudang G1:

- Suhu: 84°C
- Kelembaban: 73%
- Status: Bahaya tinggi! Barang berisiko rusak

Gudang G2:

- Suhu: 78°C
- Kelembaban: 68%
- Status: Aman

Gudang G3:

- Suhu: 85°C
- Kelembaban: 65%
- Status: Suhu tinggi, kelembaban normal

Gudang G4:

- Suhu: 79°C
- Kelembaban: 75%
- Status: Kelembaban tinggi, suhu aman

---

`4. Gabungkan Stream dari Dua Sensor`

Lakukan join antar dua stream berdasarkan gudang_id dan window waktu (misalnya 10 detik) untuk mendeteksi kondisi bahaya ganda.

---

`🎓 Tujuan Pembelajaran`

Mahasiswa diharapkan dapat:

- Memahami cara kerja Apache Kafka dalam pengolahan data real-time.
- Membuat Kafka Producer dan Consumer untuk simulasi data sensor.
- Mengimplementasikan stream filtering dengan PySpark.
- Melakukan join multi-stream dan analisis gabungan dari berbagai sensor.
- Mencetak hasil analitik berbasis kondisi kritis gudang ke dalam output console.

## Penyelesaian Soal

- Run Docker Compose

```bash
docker-compose up -d
```

![docker-compose-up](/img/docker-compose.png)

- Check Docker Compose (Container)

```bash
docker ps
docker-compose ps
```

![docker-compose-up](/img/docker-ps.png)

- Setting Kafka

```bash
docker exec -it kafka bash
```

- Making Topic

```bash
kafka-topics --create --topic sensor-suhu-gudang --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
kafka-topics --create --topic sensor-kelembaban-gudang --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

- Check Topic

```bash
kafka-topics --list --bootstrap-server localhost:9092
```

![docker-compose-up](/img/exec-kafka.png)

- Setting Pyspark

```bash
docker exec -it pyspark bash
```

- Copy Path to Pyspark

```bash
docker cp ~/desktop/kuliah/semester-4/ai/kernel/kafka pyspark:/app/
```

![docker-compose-up](/img/copy-path.png)
![docker-compose-up](/img/ls-pyspark.png)

- Setting venv

```
python3 -m venv venv
```

- Install Requirements

```bash
pip install -r requirements --no-cache-dir
```

![docker-compose-up](/img/install.png)

- Run Producer and Consumer

```bash
python temprature_producer.py
```

![docker-compose-up](/img/python-suhu.png)

```bash
python humadity_producer.py
```

![docker-compose-up](/img/exec-pyspark.png)

```bash
spark-submit \
 --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.0 \
 pyspark_consumer.py
```

![docker-compose-up](/img/kelembaban-pyspark.png)
![docker-compose-up](/img/suhu-pyspark.png)

- Stop Docker

```bash
docker-compose down -v
```

![docker-compose-up](/img/stop.png)
