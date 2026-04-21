# SauceDemo Katalon Studio - Test Automation Project

Proyek otomasi pengujian berbasis **Katalon Studio** untuk aplikasi demo e-commerce [SauceDemo](https://www.saucedemo.com). Project ini mencakup pengujian fungsional untuk fitur login dan manajemen keranjang belanja.

---

## Gambaran Umum

Project ini mengotomasi pengujian pada aplikasi **SauceDemo** (`https://www.saucedemo.com`) yang merupakan aplikasi demo untuk keperluan latihan test automation. Pengujian mencakup:

- **Login Flow**: Validasi skenario login berhasil dan gagal
- **Product Flow**: Navigasi ke detail produk dan operasi keranjang belanja

**Total Test Cases:** 8
**Total Test Suites:** 2
**Total Web Elements (Object Repository):** 39

---

## Teknologi yang Digunakan

| Teknologi | Versi |
|-----------|-------|
| Katalon Studio | Enterprise v11.1.1 |
| Groovy | 2.x |
| Selenium WebDriver | (bundled) |
| Google Chrome | 147+ |
| Java | 8+ |
| Gradle | (via wrapper) |

---

## Prasyarat

1. **Katalon Studio** versi 11.x atau lebih baru sudah terinstall
2. **Google Chrome** browser sudah terinstall
3. Koneksi internet untuk mengakses `https://www.saucedemo.com`
4. *(Opsional)* Katalon Runtime Engine untuk eksekusi via command line

---

## Struktur Project

```
SauceDemo_Katalon/
├── Test Cases/                  # Definisi test case (.tc)
│   ├── TC-01_Succed Login Using Valid Credentials.tc
│   ├── TC-02_Failed Login Using Invalid Credential.tc
│   ├── TC-03_Failed Login Without Filled Username.tc
│   ├── TC-04_Failed Login Without Filled Password.tc
│   ├── TC-05_Failed Login Without filled Username and Password.tc
│   ├── TC-06_Succed Direct To Detail Product After Login.tc
│   ├── TC-07_Succed Add Product To Cart After Login.tc
│   └── TC-08_Succed Remove Product From Cart.tc
│
├── Scripts/                     # Script Groovy untuk setiap test case
│   ├── TC-01_Succed Login Using Valid Credentials/
│   ├── TC-02_Failed Login Using Invalid Credential/
│   ├── TC-03_Failed Login Without Filled Username/
│   ├── TC-04_Failed Login Without Filled Password/
│   ├── TC-05_Failed Login Without filled Username and Password/
│   ├── TC-06_Succed Direct To Detail Product After Login/
│   ├── TC-07_Succed Add Product To Cart After Login/
│   └── TC-08_Succed Remove Product From Cart/
│
├── Test Suites/                 # Test suite untuk eksekusi terkelompok
│   ├── LG01-Login.ts            # Suite login (TC-01 s/d TC-05)
│   └── PR01-Product.ts          # Suite produk (TC-06 s/d TC-08)
│
├── Object Repository/           # Page objects / web element definitions
│   ├── TC-01_Succed.Login/
│   ├── TC-02_Failed.Login1/
│   ├── TC-03-Failed.Login2/
│   ├── TC-04_Failed.Login3/
│   ├── TC-05_Failed.Login4/
│   ├── TC-06_Product Detail/
│   ├── TC-07_Add Cart/
│   └── TC-08_Remove Cart/
│
├── Profiles/
│   └── default.glbl             # Global variables (default profile)
│
├── Keywords/                    # Custom keywords (template)
├── Reports/                     # Laporan hasil eksekusi
├── build.gradle                 # Build configuration
└── SauceDemo_Katalon.prj        # Katalon project file
```

---

## Test Cases

### Login Tests

| ID | Nama Test Case | Deskripsi | Expected Result |
|----|----------------|-----------|-----------------|
| TC-01 | Succed Login Using Valid Credentials | Login menggunakan username dan password yang valid | Berhasil login, halaman menampilkan header "Swag Labs" |
| TC-02 | Failed Login Using Invalid Credential | Login menggunakan username/password yang tidak valid | Muncul pesan error: *"Epic sadface: Username and password do not match any user in this service"* |
| TC-03 | Failed Login Without Filled Username | Login tanpa mengisi username | Muncul pesan error: *"Username is required"* |
| TC-04 | Failed Login Without Filled Password | Login tanpa mengisi password | Muncul validasi error password |
| TC-05 | Failed Login Without filled Username and Password | Login tanpa mengisi username maupun password | Muncul pesan error: *"Username is required"* |

**Kredensial yang Digunakan:**

| Tipe | Username | Password |
|------|----------|----------|
| Valid | `standard_user` | `secret_sauce` |
| Invalid | `invalid_username` | `password123` |

> Password disimpan dalam format terenkripsi menggunakan mekanisme enkripsi bawaan Katalon Studio.

---

### Product Tests

| ID | Nama Test Case | Deskripsi | Expected Result |
|----|----------------|-----------|-----------------|
| TC-06 | Succed Direct To Detail Product After Login | Navigasi ke halaman detail produk setelah login | Halaman detail produk (Sauce Labs Backpack) berhasil ditampilkan |
| TC-07 | Succed Add Product To Cart After Login | Menambahkan produk ke keranjang belanja setelah login | Produk berhasil ditambahkan, badge keranjang menampilkan angka 1 |
| TC-08 | Succed Remove Product From Cart | Menambahkan dan menghapus produk dari keranjang | Produk berhasil dihapus dari keranjang |

**Produk yang Diuji:** Sauce Labs Backpack

---

## Test Suites

### LG01-Login

Suite yang mengelompokkan semua test case login.

| Urutan Eksekusi | Test Case |
|-----------------|-----------|
| 1 | TC-04 - Failed Login Without Filled Password |
| 2 | TC-03 - Failed Login Without Filled Username |
| 3 | TC-05 - Failed Login Without filled Username and Password |
| 4 | TC-02 - Failed Login Using Invalid Credential |
| 5 | TC-01 - Succed Login Using Valid Credentials |

### PR01-Product

Suite yang mengelompokkan semua test case produk.

| Urutan Eksekusi | Test Case |
|-----------------|-----------|
| 1 | TC-08 - Succed Remove Product From Cart |
| 2 | TC-07 - Succed Add Product To Cart After Login |
| 3 | TC-06 - Succed Direct To Detail Product After Login |

**Konfigurasi Suite:**
- Page load timeout: 10 detik
- Setiap test case menggunakan instance browser terpisah (`isReuseDriver: false`)

---

## Object Repository

Semua web element didefinisikan dengan strategi seleksi XPath menggunakan atribut `data-test` dari SauceDemo.

| Element | Selector | Digunakan di |
|---------|----------|--------------|
| `input_Username` | `//input[@data-test='username']` | TC-01 s/d TC-05 |
| `input_Password` | `//input[@data-test='password']` | TC-01, TC-04 |
| `input_login-button` | `//*[@data-test='login-button']` | TC-01 s/d TC-05 |
| `h3_Epic sadface_...` | XPath error message | TC-02 |
| `div_Epic sadface_Username is required` | XPath error message | TC-03, TC-05 |
| `img_Sauce Labs Backpack` | XPath product image | TC-06 |
| `button_add-to-cart-sauce-labs-backpack` | XPath add to cart button | TC-07, TC-08 |
| `button_remove-sauce-labs-backpack` | XPath remove from cart button | TC-08 |
| `span_1` | XPath cart badge | TC-07, TC-08 |
| `div_Swag Labs` | XPath page header | TC-01 |

---

## Cara Menjalankan Test

### Via Katalon Studio GUI

1. Buka Katalon Studio
2. Buka project `SauceDemo_Katalon.prj`
3. Untuk menjalankan satu test case:
   - Buka folder **Test Cases**
   - Klik kanan pada test case yang diinginkan
   - Pilih **Run** > **Chrome**
4. Untuk menjalankan test suite:
   - Buka folder **Test Suites**
   - Klik dua kali pada `LG01-Login` atau `PR01-Product`
   - Klik tombol **Run**

### Via Command Line (Katalon Runtime Engine)

```bash
# Menjalankan Test Suite LG01-Login
katalonc -projectPath="/path/to/SauceDemo_Katalon" \
         -testSuitePath="Test Suites/LG01-Login" \
         -browserType="Chrome" \
         -executionProfile="default"

# Menjalankan Test Suite PR01-Product
katalonc -projectPath="/path/to/SauceDemo_Katalon" \
         -testSuitePath="Test Suites/PR01-Product" \
         -browserType="Chrome" \
         -executionProfile="default"
```

### Via Gradle

```bash
./gradlew katalonexec
```

---

## Hasil Pengujian

Laporan hasil pengujian tersimpan di folder `Reports/` dengan format:

- **HTML Report**: Laporan visual lengkap dengan screenshot
- **CSV**: Ringkasan eksekusi dalam format spreadsheet
- **JUnit XML**: Format standar untuk integrasi CI/CD

### Hasil Terakhir (21 April 2026)

**LG01-Login Suite:**
| Test Case | Status | Durasi |
|-----------|--------|--------|
| TC-01 | PASSED | ~3.6s |
| TC-02 | PASSED | ~2.5s |
| TC-03 | PASSED | ~2.0s |
| TC-04 | PASSED | ~1.9s |
| TC-05 | PASSED | ~2.1s |
| **Total** | **5/5 PASSED** | **14.0s** |

Browser yang digunakan: Chrome 147.0.7727.56

---

## Catatan

- Semua password dienkripsi menggunakan fitur enkripsi bawaan Katalon Studio
- Project menggunakan Katalon Enterprise Edition v11.1.1
- Custom keywords belum diimplementasikan (masih template)
- Global variables masih kosong, credentials langsung di-hardcode dalam script
