# Cheatsheet UTS Mobile Programming (Android Studio, Java)

> Waktu cuma 2 jam, yang penting jalan dulu, baru rapihin.
> Urutan materi = urutan di PDF Notes (yang paling sering keluar): **Folder → Layout → View → Methods → Intent → Menu → RecyclerView**.
> Contoh kode diambil dari latihan: `sportshop`, `Elektronik`, `restoran`, `Sesi6`.

## Daftar Isi

0. [Alur Ngerjain Ujian](#0-alur-ngerjain-ujian)
1. [Folder & Cara Bikin File](#1-folder--cara-bikin-file)
2. [Template Activity](#2-template-activity-jangan-dihapus)
3. [Layout](#3-layout)
4. [View](#4-view)
5. [Methods: findViewById, OnClick, OnFocus, Toast](#5-methods)
6. [Intent: pindah page & kirim data](#6-intent)
7. [Menu (titik tiga)](#7-menu-titik-tiga-di-kanan-atas)
8. [RecyclerView (6 langkah)](#8-recyclerview-6-langkah)
9. [Form Input: RadioGroup, CheckBox, Spinner, AlertDialog](#9-form-input-restoran)
10. [Pola Siap Pakai: Profile, Calculator, Chat](#10-pola-siap-pakai)
11. [Error Umum & Cara Fix](#11-error-umum--cara-fix)
12. [Daftar Import](#12-daftar-import)

---

## 0. Alur Ngerjain Ujian

1. **New Project** → *Empty Views Activity* → Language **Java** → Finish.
2. Copy semua gambar ke `res/drawable/`.
3. Bikin **layout XML** dulu (tampilan), kasih `android:id` ke semua yang mau diotak-atik di Java.
4. Bikin **Activity Java**: `findViewById` → pasang listener.
5. Kalau ada titik tiga: bikin **menu** + **Toolbar** (bagian 7).
6. Kalau ada list: bikin **RecyclerView** (bagian 8).
7. Run sesering mungkin. Jangan nunggu semua kelar baru run.

**Checklist sebelum submit:**

- [ ] Semua Activity baru sudah terdaftar di `AndroidManifest.xml`
- [ ] Root layout tiap activity punya `android:id="@+id/main"`
- [ ] Semua `Toast` ada `.show()`
- [ ] RecyclerView sudah `setLayoutManager` **dan** `setAdapter`
- [ ] Nama gambar huruf kecil semua, tanpa spasi

---

## 1. Folder & Cara Bikin File

| Folder | Isinya |
|---|---|
| `java/id.ac.binus.namaapp/` | File Java (logic): Activity, Adapter, ViewHolder, class data |
| `res/drawable/` | Gambar (`sepatu1.jpeg`, `logo.png`) |
| `res/layout/` | File XML tampilan (`activity_main.xml`, `item_produk.xml`) |
| `res/menu/` | File menu titik tiga (`menu_main.xml`) |
| `res/values/` | `strings.xml` (teks & string-array), `themes.xml`, `colors.xml` |
| `manifests/AndroidManifest.xml` | Daftar semua Activity |

**Cara bikin:**

| Mau bikin | Caranya |
|---|---|
| Activity baru (Java + XML + auto daftar Manifest) | Klik kanan folder package → **New → Activity → Empty Views Activity** |
| Class biasa (data / Adapter / ViewHolder) | Klik kanan folder package → **New → Java Class** |
| Layout untuk item list | Klik kanan `res/layout` → **New → Layout Resource File** |
| Folder menu | Klik kanan `res` → **New → Android Resource Directory** → Resource type: **menu** → OK |
| File menu | Klik kanan `res/menu` → **New → Menu Resource File** |
| Gambar | Copy file → klik kanan `res/drawable` → Paste |

> **Nama gambar:** huruf kecil, angka, underscore aja. `Sepatu 1.jpg` ❌ → `sepatu1.jpg` ✅. Gak boleh diawali angka.
> **Semua file Java taruh di folder package yang sama** (yang ada `MainActivity`). Jangan di luar folder package.

---

## 2. Template Activity (jangan dihapus)

Ini otomatis dibikin Android Studio. Kode kamu taruh **di bawah** blok `ViewCompat`.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        // ===== KODE KAMU MULAI DI SINI =====
    }
}
```

> ⚠️ `findViewById(R.id.main)` artinya **root layout XML wajib punya `android:id="@+id/main"`**. Kalau layout diganti (misal ke LinearLayout / ScrollView) dan id-nya hilang → app langsung crash.

**Manifest** (kalau bikin Activity lewat *New → Java Class*, harus daftar manual):

```xml
<activity
    android:name=".ProfileActivity"
    android:exported="false" />
```

---

## 3. Layout

**Ukuran:** `textSize` pakai **sp**, sisanya (width, height, margin, padding) pakai **dp**.
`match_parent` = selebar/setinggi parent. `wrap_content` = secukupnya isi.

### A. LinearLayout (paling sering dipakai, kayak flexbox)

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_horizontal"
    android:padding="16dp">

    <!-- isi di sini, tersusun dari atas ke bawah -->

</LinearLayout>
```

| Atribut | Fungsi |
|---|---|
| `orientation="vertical"` / `"horizontal"` | Susun atas-bawah / kiri-kanan |
| `gravity="center"` | Posisi **isi** di dalam layout (center, center_horizontal, start, end) |
| `layout_gravity="center"` | Posisi **view itu sendiri** di dalam parent |
| `layout_weight="1"` + `layout_width="0dp"` | Bagi lebar sama rata |

**Tombol sejajar lebar sama** (dipakai di calculator & restoran):

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <Button
        android:id="@+id/btnBatal"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Batal" />

    <Button
        android:id="@+id/btnPesan"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Pesan" />
</LinearLayout>
```

### B. RelativeLayout

Atur posisi relatif ke view lain: `layout_toStartOf`, `layout_toEndOf`, `layout_below`, `layout_above`, `layout_alignParentTop`, `layout_centerInParent`.

```xml
<TextView android:id="@+id/tvJudul" ... android:layout_alignParentTop="true" />
<TextView ... android:layout_below="@id/tvJudul" />
```

### C. ConstraintLayout (default dari template)

Mirip Relative. **Kalau lupa caranya, drag & drop aja di tab Design, yang penting nampak.** Tiap view minimal di-constraint atas & kiri.

```xml
app:layout_constraintTop_toTopOf="parent"
app:layout_constraintStart_toStartOf="parent"
app:layout_constraintEnd_toEndOf="parent"
app:layout_constraintTop_toBottomOf="@id/navMenu"
```

> Di ConstraintLayout, `layout_width="0dp"` artinya "ikut constraint" (melebar dari Start ke End).

### D. ScrollView (kalau isi kepanjangan)

ScrollView **cuma boleh punya 1 anak**, jadi bungkus pakai LinearLayout:

```xml
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <!-- isi form panjang di sini -->

    </LinearLayout>
</ScrollView>
```

---

## 4. View

| View | Fungsi | Atribut penting |
|---|---|---|
| `TextView` | Tampilin tulisan | `text`, `textSize="18sp"`, `textStyle="bold"`, `textAlignment`, `gravity` |
| `EditText` | User ngetik | `hint`, `inputType="number"` / `"text"` / `"textPassword"` |
| `Button` | Tombol | `text` |
| `ImageView` | Tampilin gambar | `src="@drawable/nama"`, `scaleType="centerCrop"`, `contentDescription` |
| `Toolbar` | Bar atas buat menu titik tiga | `androidx.appcompat.widget.Toolbar` |
| `RecyclerView` | List yang bisa scroll | `androidx.recyclerview.widget.RecyclerView` |
| `RadioGroup` + `RadioButton` | Pilih **satu** | `checked="true"` buat default |
| `CheckBox` | Pilih **banyak** | `text` |
| `Spinner` | Dropdown | `entries="@array/nama_array"` |
| `CardView` | Kotak/lingkaran | `app:cardCornerRadius`, `app:cardElevation` |

Contoh lengkap:

```xml
<TextView
    android:id="@+id/tvHasil"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="16dp"
    android:text="Hasil: "
    android:textSize="22sp"
    android:textStyle="bold" />

<EditText
    android:id="@+id/etInput1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Angka 1"
    android:inputType="number" />

<ImageView
    android:layout_width="match_parent"
    android:layout_height="200dp"
    android:scaleType="centerCrop"
    android:src="@drawable/logo"
    android:contentDescription="Logo" />
```

---

## 5. Methods

### A. findViewById

Buat ambil view dari XML ke Java. **Kasih `android:id` di XML dulu.**

```java
EditText etMessage = findViewById(R.id.etMessage);
TextView tvResult  = findViewById(R.id.tvResult);
ImageView imgView  = findViewById(R.id.imgView);

String text = etMessage.getText().toString().trim();   // baca EditText
tvResult.setText("Result: " + text);                    // tulis TextView
imgView.setImageResource(R.drawable.sepatu1);           // ganti gambar

int angka = Integer.parseInt(text);                     // String -> int
double desimal = Double.parseDouble(text);              // String -> double
```

> Kalau variabelnya dipakai di banyak method, deklarasiin di atas (di luar `onCreate`), terus isi di `onCreate`:
>
> ```java
> public class MainActivity extends AppCompatActivity {
>     EditText etInput1, etInput2;
>     TextView tvHasil;
>     // ... di onCreate: etInput1 = findViewById(R.id.etInput1);
> ```

### B. OnClickListener

```java
btnKirim.setOnClickListener(v -> {
    // kode kalau tombol diklik
});

btnTambah.setOnClickListener(v -> hitung("+"));   // panggil method
btnKembali.setOnClickListener(v -> finish());     // tutup activity, balik ke sebelumnya
```

### C. OnFocusChangeListener

Buat tau EditText mana yang lagi aktif (dipakai di calculator tombol Clear).

```java
EditText currentInput;   // deklarasi di atas

etInput1.setOnFocusChangeListener((view, hasFocus) -> {
    if (hasFocus) currentInput = etInput1;
});
etInput2.setOnFocusChangeListener((view, hasFocus) -> {
    if (hasFocus) currentInput = etInput2;
});

btnClear.setOnClickListener(v -> {
    if (currentInput != null) currentInput.setText("");
});
```

### D. Toast

Popup kecil di bawah layar.

```java
Toast.makeText(this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
```

> **Jangan lupa `.show()`**, kalau gak, gak muncul.
> Di dalam **Adapter**, ganti `this` jadi `context`.

**Validasi input kosong** (pola yang sering dipakai):

```java
String jumlahText = edtJumlah.getText().toString().trim();
if (jumlahText.isEmpty()) {
    Toast.makeText(this, "Jumlah harus diisi!", Toast.LENGTH_SHORT).show();
    return;   // stop di sini
}
```

---

## 6. Intent

Dipakai buat **pindah page** dan **kirim data**.

### Pindah page

```java
Intent intent = new Intent(this, ProfileActivity.class);   // (dari, ke)
startActivity(intent);

// versi 1 baris
startActivity(new Intent(this, ProfileActivity.class));
```

### Kirim data (A → B) — `putExtra` **sebelum** `startActivity`

```java
Intent intent = new Intent(this, ProductActivity.class);
intent.putExtra("Category", "Shoes");          // ("KEY", value)
intent.putExtra("gambar", R.drawable.laptop1); // int juga bisa
startActivity(intent);
```

### Terima data (di B) — **pakai key yang sama persis** (huruf besar-kecil ngaruh)

```java
String category = getIntent().getStringExtra("Category");
int gambar      = getIntent().getIntExtra("gambar", 0);   // 0 = nilai default

// cek dulu ada datanya atau gak
if (getIntent().hasExtra("reply")) {
    String reply = getIntent().getStringExtra("reply");
}
```

### Balik ke page sebelumnya

```java
finish();
```

---

## 7. Menu (titik tiga di kanan atas)

### Langkah 1 — Bikin folder `res/menu`

Klik kanan `res` → New → Android Resource Directory → Resource type **menu** → OK.

### Langkah 2 — Bikin `res/menu/menu_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/navProfile"
        android:title="Profile" />

    <item
        android:id="@+id/navCalculator"
        android:title="Calculator" />

    <!-- menu di dalam menu (submenu) -->
    <item
        android:id="@+id/navProduct"
        android:title="Product">
        <menu>
            <item
                android:id="@+id/navShoes"
                android:title="Shoes" />
            <item
                android:id="@+id/navBall"
                android:title="Ball" />
        </menu>
    </item>
</menu>
```

### Langkah 3 — Taruh Toolbar di `activity_main.xml`

LinearLayout:

```xml
<androidx.appcompat.widget.Toolbar
    android:id="@+id/navMenu"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize" />
```

ConstraintLayout:

```xml
<androidx.appcompat.widget.Toolbar
    android:id="@+id/navMenu"
    android:layout_width="0dp"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintEnd_toEndOf="parent" />
```

### Langkah 4 — Munculin Toolbar di `MainActivity.java`

```java
import androidx.appcompat.widget.Toolbar;   // ⚠️ Toolbar ada 2, PILIH YANG INI

public class MainActivity extends AppCompatActivity {

    Toolbar navMenu;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ... template ...
        navMenu = findViewById(R.id.navMenu);
        setSupportActionBar(navMenu);   // toolbar muncul, tapi BELUM ada titik tiga
    }
```

### Langkah 5 — Munculin titik tiga (belum bisa diklik)

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.menu_main, menu);   // ganti sesuai nama file menu
    return true;
}
```

### Langkah 6 — Biar isinya bisa diklik

```java
@Override
public boolean onOptionsItemSelected(@NonNull MenuItem item) {
    int id = item.getItemId();

    if (id == R.id.navProfile) {
        startActivity(new Intent(this, ProfileActivity.class));
        return true;
    } else if (id == R.id.navShoes) {
        Intent intent = new Intent(this, ProductActivity.class);
        intent.putExtra("Category", "Shoes");
        startActivity(intent);
        return true;
    } else if (id == R.id.navBall) {
        Intent intent = new Intent(this, ProductActivity.class);
        intent.putExtra("Category", "Ball");
        startActivity(intent);
        return true;
    }
    return super.onOptionsItemSelected(item);
}
```

> **Tips:** ketik `onCreateOptionsMenu` / `onOptionsItemSelected` di dalam class, tekan Enter, Android Studio auto-generate.
> Item yang punya submenu (misal `navProduct`) gak perlu di-handle, cukup anak-anaknya.

---

## 8. RecyclerView (6 Langkah)

Container berisi banyak item yang **di-reuse**. Kalau layar muat 10 card, dia cuma bikin ±10 card; pas scroll, card yang hilang di atas dipakai lagi di bawah.

**Urutan file:** `item_produk.xml` → RecyclerView di activity XML → `Produk.java` → `ProdukViewHolder.java` → `ProdukAdapter.java` → sambungin di Activity.

### Langkah 1 — `res/layout/item_produk.xml` (tampilan 1 card)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="8dp"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/imgProduk"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:scaleType="centerCrop" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:orientation="vertical">

        <TextView
            android:id="@+id/txtNama"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            android:textStyle="bold" />

        <TextView
            android:id="@+id/txtHarga"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />

        <TextView
            android:id="@+id/txtDeskripsi"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </LinearLayout>
</LinearLayout>
```

> ⚠️ Root item **harus** `layout_height="wrap_content"`. Kalau `match_parent`, 1 item makan 1 layar penuh.

### Langkah 2 — Pasang RecyclerView di activity XML

```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/rvProduk"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

### Langkah 3 — Class data `Produk.java`

Gambar di Android tipenya **int** (`R.drawable.xxx`).

```java
public class Produk {
    private int gambar;
    private String nama;
    private String harga;
    private String deskripsi;

    public Produk(int gambar, String nama, String harga, String deskripsi) {
        this.gambar = gambar;
        this.nama = nama;
        this.harga = harga;
        this.deskripsi = deskripsi;
    }

    public int getGambar() { return gambar; }
    public String getNama() { return nama; }
    public String getHarga() { return harga; }
    public String getDeskripsi() { return deskripsi; }
}
```

> **Cepat:** tulis 4 variabel `private` → klik kanan → **Generate** (Alt+Insert) → **Constructor** (pilih semua) → lalu **Getter** (pilih semua).

### Langkah 4 — `ProdukViewHolder.java`

Nyimpen referensi semua view di `item_produk.xml`.

```java
public class ProdukViewHolder extends RecyclerView.ViewHolder {
    ImageView imgProduk;
    TextView txtNama, txtHarga, txtDeskripsi;

    public ProdukViewHolder(@NonNull View itemView) {
        super(itemView);
        imgProduk    = itemView.findViewById(R.id.imgProduk);
        txtNama      = itemView.findViewById(R.id.txtNama);
        txtHarga     = itemView.findViewById(R.id.txtHarga);
        txtDeskripsi = itemView.findViewById(R.id.txtDeskripsi);
    }
}
```

> ⚠️ Pakai **`itemView.findViewById`**, bukan `findViewById` biasa.
> ⚠️ Cocokin variabel ↔ id dengan benar. Contoh di PDF ada yang **ketuker** (`tvShoesDesc = ...tvShoesName`, dst). Jangan dicontek mentah.

### Langkah 5 — `ProdukAdapter.java`

Adapter = penghubung **data** ↔ **item view**. Butuh: ViewHolder, data (ArrayList), Context.

```java
public class ProdukAdapter extends RecyclerView.Adapter<ProdukViewHolder> {
    Context context;
    ArrayList<Produk> dataProduk;

    public ProdukAdapter(Context context, ArrayList<Produk> dataProduk) {
        this.context = context;
        this.dataProduk = dataProduk;
    }

    // 1. Bikin tampilan 1 item (ubah XML jadi View)
    @NonNull
    @Override
    public ProdukViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        return new ProdukViewHolder(
                LayoutInflater.from(context).inflate(R.layout.item_produk, parent, false));
    }

    // 2. Isi data ke tampilan
    @Override
    public void onBindViewHolder(@NonNull ProdukViewHolder holder, int position) {
        Produk produk = dataProduk.get(position);
        holder.imgProduk.setImageResource(produk.getGambar());
        holder.txtNama.setText(produk.getNama());
        holder.txtHarga.setText(produk.getHarga());
        holder.txtDeskripsi.setText(produk.getDeskripsi());
    }

    // 3. Jumlah data
    @Override
    public int getItemCount() {
        return dataProduk.size();
    }
}
```

> **Cepat:** tulis `extends RecyclerView.Adapter<ProdukViewHolder>` → hover merah → **Implement methods** → pilih semua.
> **Hafal mati 1 baris ini:** `LayoutInflater.from(context).inflate(R.layout.item_produk, parent, false)`
> `false` = jangan langsung dimasukin ke parent, biar RecyclerView yang ngurus.

| Method | Kapan dipanggil |
|---|---|
| `onCreateViewHolder` | RecyclerView butuh card baru (cuma sebanyak yang muat di layar) |
| `onBindViewHolder` | Isi data posisi ke-`position` ke card |
| `getItemCount` | RecyclerView nanya ada berapa data |

### Langkah 6 — Sambungin di Activity

```java
RecyclerView rvProduk;   // di atas onCreate

// di dalam onCreate, setelah template:
ArrayList<Produk> dataProduk = new ArrayList<>();
dataProduk.add(new Produk(R.drawable.sepatu1, "Nike Air Max", "Rp 1.800.000", "Sepatu lari empuk"));
dataProduk.add(new Produk(R.drawable.sepatu2, "Adidas Ultraboost", "Rp 2.500.000", "Ringan dan responsif"));

rvProduk = findViewById(R.id.rvProduk);
rvProduk.setLayoutManager(new LinearLayoutManager(this));   // WAJIB, kalau lupa list kosong
rvProduk.setAdapter(new ProdukAdapter(this, dataProduk));
```

| LayoutManager | Hasil |
|---|---|
| `new LinearLayoutManager(this)` | List vertikal (default) |
| `new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)` | List horizontal |
| `new GridLayoutManager(this, 2)` | Grid 2 kolom |

### Variasi A — Klik item → Toast (di `onBindViewHolder`)

```java
holder.itemView.setOnClickListener(v ->
        Toast.makeText(context, "You choose " + produk.getNama(), Toast.LENGTH_SHORT).show());
```

### Variasi B — Klik item → buka Detail + kirim data (di `onBindViewHolder`)

```java
holder.itemView.setOnClickListener(v -> {
    Intent intent = new Intent(context, DetailActivity.class);
    intent.putExtra("gambar", produk.getGambar());
    intent.putExtra("nama", produk.getNama());
    intent.putExtra("harga", produk.getHarga());
    intent.putExtra("deskripsi", produk.getDeskripsi());
    context.startActivity(intent);   // di Adapter pakai context.startActivity
});
```

`DetailActivity.java`:

```java
int gambar       = getIntent().getIntExtra("gambar", 0);
String nama      = getIntent().getStringExtra("nama");
String harga     = getIntent().getStringExtra("harga");
String deskripsi = getIntent().getStringExtra("deskripsi");

ImageView imgDetail = findViewById(R.id.imgDetail);
TextView txtNama    = findViewById(R.id.txtNamaDetail);
// ... dst

imgDetail.setImageResource(gambar);
txtNama.setText(nama);
findViewById(R.id.btnKembali).setOnClickListener(v -> finish());
```

### Variasi C — Kategori beda pakai 1 Activity (dari menu, kirim key)

`MainActivity` kirim `intent.putExtra("Category", "Shoes")`, lalu di `ProductActivity`:

```java
String category = getIntent().getStringExtra("Category");
ArrayList<Product> dataProduct = new ArrayList<>();

if (category.equals("Shoes")) {
    dataProduct.add(new Product(R.drawable.sepatu1, "Nike Air Max", "Rp 1.800.000", "Sepatu lari"));
} else if (category.equals("Ball")) {
    dataProduct.add(new Product(R.drawable.bola1, "Bola Sepak", "Rp 350.000", "Ukuran 5"));
}
```

> Bandingin String pakai **`.equals()`**, bukan `==`.

### Variasi D — Ganti kategori di halaman yang sama (cukup ganti adapter)

```java
ArrayList<Produk> dataLaptop = new ArrayList<>();   // di atas onCreate
ArrayList<Produk> dataMouse  = new ArrayList<>();

// di onOptionsItemSelected:
if (id == R.id.navLaptop) {
    rvProduk.setAdapter(new ProdukAdapter(this, dataLaptop));
    return true;
} else if (id == R.id.navMouse) {
    rvProduk.setAdapter(new ProdukAdapter(this, dataMouse));
    return true;
}
```

---

## 9. Form Input (restoran)

### XML

```xml
<RadioGroup
    android:id="@+id/rgPorsi"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <RadioButton android:id="@+id/rbKecil"  android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Kecil" />
    <RadioButton android:id="@+id/rbSedang" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Sedang" android:checked="true" />
    <RadioButton android:id="@+id/rbBesar"  android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Besar" />
</RadioGroup>

<CheckBox
    android:id="@+id/cbSambal"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Tambah sambal" />

<Spinner
    android:id="@+id/spBayar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:entries="@array/metode_bayar" />
```

`res/values/strings.xml` (isi Spinner):

```xml
<string-array name="metode_bayar">
    <item>Tunai</item>
    <item>QRIS</item>
    <item>Debit</item>
</string-array>
```

### Java — ambil nilai

```java
// RadioGroup -> teks yang dipilih
RadioButton rbDipilih = findViewById(rgPorsi.getCheckedRadioButtonId());
String porsi = rbDipilih.getText().toString();

// CheckBox
String tambahan = "";
if (cbSambal.isChecked())  tambahan += "Sambal ";
if (cbBungkus.isChecked()) tambahan += "Bungkus";
if (tambahan.isEmpty())    tambahan = "-";

// Spinner
String metode = spBayar.getSelectedItem().toString();

// Hitung
int total = 25000 * Integer.parseInt(edtJumlah.getText().toString());
```

### AlertDialog (popup konfirmasi)

```java
new AlertDialog.Builder(this)
        .setTitle("Konfirmasi Pesanan")
        .setMessage("Porsi: " + porsi + "\nBayar: " + metode + "\n\nTotal: Rp " + total)
        .setPositiveButton("Ya", (dialog, which) ->
                Toast.makeText(this, "Pesanan berhasil!", Toast.LENGTH_SHORT).show())
        .setNegativeButton("Batal", null)
        .show();
```

> Import yang benar: `androidx.appcompat.app.AlertDialog`. `\n` = baris baru.

### Reset form (tombol Batal)

```java
edtJumlah.setText("");
rgPorsi.check(R.id.rbSedang);
cbSambal.setChecked(false);
spBayar.setSelection(0);
```

---

## 10. Pola Siap Pakai

### Profile dengan foto bulat (CardView)

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal"
    android:orientation="vertical"
    android:paddingTop="48dp">

    <androidx.cardview.widget.CardView
        android:layout_width="150dp"
        android:layout_height="150dp"
        app:cardCornerRadius="75dp"
        app:cardElevation="0dp">

        <ImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scaleType="centerCrop"
            android:src="@drawable/profil" />
    </androidx.cardview.widget.CardView>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:text="Vinson Mervin"
        android:textSize="22sp"
        android:textStyle="bold" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="NIM : 2802407263"
        android:textSize="16sp" />

    <Button
        android:id="@+id/btnKembali"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="Kembali" />
</LinearLayout>
```

> Lingkaran: `cardCornerRadius` = **setengah** dari width/height (150 → 75).
> Java-nya cuma: `findViewById(R.id.btnKembali).setOnClickListener(v -> finish());`

### Calculator (2 input + 4 operator + Clear)

```java
private void hitung(String operator) {
    String text1 = etInput1.getText().toString().trim();
    String text2 = etInput2.getText().toString().trim();

    if (text1.isEmpty() || text2.isEmpty()) {
        Toast.makeText(this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
        return;
    }

    double angka1 = Double.parseDouble(text1);
    double angka2 = Double.parseDouble(text2);
    double hasil = 0;

    if (operator.equals("+"))      hasil = angka1 + angka2;
    else if (operator.equals("-")) hasil = angka1 - angka2;
    else if (operator.equals("*")) hasil = angka1 * angka2;
    else if (operator.equals("/")) {
        if (angka2 == 0) {
            Toast.makeText(this, "Tidak bisa dibagi 0", Toast.LENGTH_SHORT).show();
            return;
        }
        hasil = angka1 / angka2;
    }
    tvHasil.setText("Hasil: " + hasil);
}

// di onCreate:
btnTambah.setOnClickListener(v -> hitung("+"));
btnKurang.setOnClickListener(v -> hitung("-"));
btnKali.setOnClickListener(v -> hitung("*"));
btnBagi.setOnClickListener(v -> hitung("/"));
```

Tombol Clear pakai OnFocus → lihat [5C](#c-onfocuschangelistener).

### Chat bolak-balik (Message ⇄ Reply)

`MessageActivity`:

```java
if (getIntent().hasExtra("reply")) {
    tvHistory.setText("Balasan: " + getIntent().getStringExtra("reply"));
}

btnKirim.setOnClickListener(v -> {
    String message = etMessage.getText().toString().trim();
    if (message.isEmpty()) {
        Toast.makeText(this, "Pesan tidak boleh kosong", Toast.LENGTH_SHORT).show();
        return;
    }
    Intent intent = new Intent(this, ReplyActivity.class);
    intent.putExtra("message", message);
    startActivity(intent);
});
```

`ReplyActivity`:

```java
tvPesanMasuk.setText("Pesan masuk: " + getIntent().getStringExtra("message"));

btnBalas.setOnClickListener(v -> {
    Intent intent = new Intent(this, MessageActivity.class);
    intent.putExtra("reply", etReply.getText().toString().trim());
    startActivity(intent);
});
```

---

## 11. Error Umum & Cara Fix

| Gejala | Penyebab | Fix |
|---|---|---|
| App langsung **force close** pas buka page | Root layout gak punya `android:id="@+id/main"` | Tambahin `android:id="@+id/main"` di root layout |
| Crash pas pindah page: `Unable to find explicit activity class` | Activity belum didaftar | Tambah `<activity android:name=".NamaActivity" />` di Manifest |
| Crash `This Activity already has an action bar` | Theme masih punya ActionBar | `res/values/themes.xml` → parent `Theme.Material3.DayNight.NoActionBar` |
| `setSupportActionBar` merah | Salah import Toolbar | Pakai `import androidx.appcompat.widget.Toolbar;` |
| Titik tiga gak muncul | Lupa `setSupportActionBar` atau `onCreateOptionsMenu` | Cek langkah 4 & 5 bagian Menu |
| RecyclerView **kosong** | Lupa `setLayoutManager` / data kosong / `getItemCount` return 0 | Cek langkah 6 & `return data.size();` |
| 1 item makan 1 layar | Root `item_xxx.xml` height `match_parent` | Ganti `wrap_content` |
| Crash `NullPointerException` di `setText` | Id salah / view ada di layout lain | Cocokin id XML ↔ `R.id.xxx`; di ViewHolder pakai `itemView.findViewById` |
| Toast gak muncul | Lupa `.show()` | Tambah `.show()` |
| Crash `NumberFormatException` | `parseInt` string kosong | Validasi `isEmpty()` dulu + `inputType="number"` |
| Data intent `null` | Key beda (huruf besar-kecil) | Key `putExtra` = key `getStringExtra` |
| `R` merah semua | Ada error di file XML / nama gambar salah | Cek XML yang merah, rename gambar huruf kecil, lalu **Build → Rebuild Project** |
| Nama gambar error | Ada spasi / huruf besar / diawali angka | Rename: `sepatu1.jpg` |

---

## 12. Daftar Import

> Biasanya cukup klik kata yang merah → **Alt+Enter** → Import class.

```java
// Dasar
import android.content.Context;
import android.content.Intent;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;

// Widget
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;

// AndroidX
import androidx.annotation.NonNull;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;          // ⚠️ yang ini, bukan android.widget.Toolbar
import androidx.recyclerview.widget.GridLayoutManager;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import java.util.ArrayList;
```

### Shortcut Android Studio

| Shortcut | Fungsi |
|---|---|
| `Alt + Enter` | Fix merah / import / implement methods |
| `Alt + Insert` | Generate constructor, getter, setter, override |
| `Ctrl + O` | Override method (onCreateOptionsMenu, dll) |
| `Ctrl + Alt + L` | Rapihin kode |
| `Shift + F10` | Run app |
| `Ctrl + /` | Comment / uncomment |
