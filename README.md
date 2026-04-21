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



# Dokumentasi Teknis - SauceDemo Katalon Test Automation

## Daftar Isi

1. [Arsitektur Project](#1-arsitektur-project)
2. [Alur Pengujian (Test Flow)](#2-alur-pengujian-test-flow)
3. [Detail Script Groovy](#3-detail-script-groovy)
4. [Object Repository Detail](#4-object-repository-detail)
5. [Konfigurasi Project](#5-konfigurasi-project)
6. [Panduan Pengembangan](#6-panduan-pengembangan)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Arsitektur Project

Project ini menggunakan arsitektur **Record & Playback** dengan pendekatan **Page Object** melalui Object Repository Katalon.

```
┌─────────────────────────────────────────────────┐
│                  TEST SUITES                     │
│          LG01-Login | PR01-Product               │
└──────────────────┬──────────────────────────────┘
                   │ mengelompokkan
┌──────────────────▼──────────────────────────────┐
│                 TEST CASES                       │
│   TC-01 | TC-02 | TC-03 | ... | TC-08           │
└──────────────────┬──────────────────────────────┘
                   │ diimplementasikan oleh
┌──────────────────▼──────────────────────────────┐
│                  SCRIPTS                         │
│           Script Groovy (.groovy)                │
└──────────────────┬──────────────────────────────┘
                   │ menggunakan
         ┌─────────┴──────────┐
         │                    │
┌────────▼───────┐   ┌────────▼────────┐
│ OBJECT REPO    │   │ KATALON WEBUI   │
│ Web Elements   │   │ Keywords Built-in│
│ (.rs files)    │   │ (WebUI.click,   │
│                │   │  WebUI.setText) │
└────────────────┘   └─────────────────┘
```

### Komponen Utama

| Komponen | Deskripsi | Lokasi |
|----------|-----------|--------|
| Test Cases | Definisi skenario pengujian | `Test Cases/*.tc` |
| Scripts | Implementasi Groovy dari test case | `Scripts/*/Script*.groovy` |
| Test Suites | Pengelompokan test case | `Test Suites/*.ts` |
| Object Repository | Definisi web element | `Object Repository/**/*.rs` |
| Profiles | Konfigurasi variabel global | `Profiles/*.glbl` |
| Keywords | Custom keyword (belum diisi) | `Keywords/` |

---

## 2. Alur Pengujian (Test Flow)

### Flow Login Berhasil (TC-01)

```
Start
  │
  ▼
Buka Browser
  │
  ▼
Navigasi ke https://www.saucedemo.com
  │
  ▼
Input Username: "standard_user"
  │
  ▼
Input Password: "secret_sauce" (encrypted)
  │
  ▼
Klik Tombol Login
  │
  ▼
Verifikasi: Header halaman = "Swag Labs"
  │
  ▼
Tutup Browser
  │
  ▼
End (PASSED)
```

### Flow Login Gagal - Invalid Credential (TC-02)

```
Start
  │
  ▼
Buka Browser → Navigasi ke URL
  │
  ▼
Input Username: "invalid_username"
Input Password: "password123" (encrypted)
  │
  ▼
Klik Tombol Login
  │
  ▼
Verifikasi: Error Message =
"Epic sadface: Username and password do not match any user in this service"
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Login Gagal - Username Kosong (TC-03)

```
Start → Buka Browser → Navigasi ke URL
  │
  ▼
Input Password saja (Username dibiarkan kosong)
  │
  ▼
Klik Tombol Login
  │
  ▼
Verifikasi: Error Message = "Username is required"
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Login Gagal - Password Kosong (TC-04)

```
Start → Buka Browser → Navigasi ke URL
  │
  ▼
Input Username saja (Password dibiarkan kosong)
  │
  ▼
Klik Tombol Login
  │
  ▼
Verifikasi: Error validasi password muncul
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Login Gagal - Kedua Field Kosong (TC-05)

```
Start → Buka Browser → Navigasi ke URL
  │
  ▼
Langsung Klik Tombol Login (tanpa isi apapun)
  │
  ▼
Verifikasi: Error Message = "Username is required"
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Navigasi Detail Produk (TC-06)

```
Start → Login dengan kredensial valid
  │
  ▼
Klik gambar produk "Sauce Labs Backpack"
  │
  ▼
Verifikasi: Halaman detail produk terbuka
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Tambah ke Keranjang (TC-07)

```
Start → Login dengan kredensial valid
  │
  ▼
Klik tombol "Add to Cart" pada Sauce Labs Backpack
  │
  ▼
Verifikasi: Badge keranjang menampilkan angka "1"
  │
  ▼
Klik badge keranjang → Navigasi ke halaman Cart
  │
  ▼
Verifikasi: Produk terlihat di keranjang
  │
  ▼
Tutup Browser → End (PASSED)
```

### Flow Hapus dari Keranjang (TC-08)

```
Start → Login dengan kredensial valid
  │
  ▼
Klik "Add to Cart" → Klik badge keranjang
  │
  ▼
Klik tombol "Remove" di halaman keranjang
  │
  ▼
Kembali ke halaman produk
  │
  ▼
Klik "Add to Cart" lagi → Navigasi ke keranjang
  │
  ▼
Klik tombol "Remove" sekali lagi
  │
  ▼
Verifikasi: Keranjang kosong
  │
  ▼
Tutup Browser → End (PASSED)
```

---

## 3. Detail Script Groovy

### Struktur Standar Script

Setiap script test case mengikuti pola berikut:

```groovy
// Import wajib
import static com.kms.katalon.core.checkpoint.CheckpointFactory.findCheckpoint
import static com.kms.katalon.core.testcase.TestCaseFactory.findTestCase
import static com.kms.katalon.core.testdata.TestDataFactory.findTestData
import static com.kms.katalon.core.testobject.ObjectRepository.findTestObject
import static com.kms.katalon.core.testobject.ObjectRepository.findTestObjectAbsolutePath
import com.kms.katalon.core.configuration.RunConfiguration as RunConfiguration
import com.kms.katalon.core.model.FailureHandling as FailureHandling
import com.kms.katalon.core.testcase.TestCase as TestCase
import com.kms.katalon.core.testdata.TestData as TestData
import com.kms.katalon.core.testobject.TestObject as TestObject
import com.kms.katalon.core.webui.keyword.WebUiBuiltInKeywords as WebUI
import org.openqa.selenium.Keys as Keys

// Body test
WebUI.openBrowser('')
WebUI.navigateToUrl('https://www.saucedemo.com')

// Aksi pengujian...

WebUI.closeBrowser()
```

### Keywords yang Digunakan

| Keyword | Deskripsi | Contoh Penggunaan |
|---------|-----------|-------------------|
| `WebUI.openBrowser('')` | Membuka browser baru | `WebUI.openBrowser('')` |
| `WebUI.navigateToUrl(url)` | Navigasi ke URL | `WebUI.navigateToUrl('https://www.saucedemo.com')` |
| `WebUI.setText(obj, text)` | Mengisi teks pada input | `WebUI.setText(findTestObject('...'), 'standard_user')` |
| `WebUI.setEncryptedText(obj, text)` | Mengisi teks terenkripsi (password) | `WebUI.setEncryptedText(findTestObject('...'), 'encryptedPass')` |
| `WebUI.click(obj)` | Mengklik elemen | `WebUI.click(findTestObject('...'))` |
| `WebUI.verifyElementText(obj, text)` | Verifikasi teks elemen | `WebUI.verifyElementText(findTestObject('...'), 'Swag Labs')` |
| `WebUI.closeBrowser()` | Menutup browser | `WebUI.closeBrowser()` |

### Contoh Script Lengkap: TC-01

```groovy
// TC-01: Berhasil Login dengan Kredensial Valid

WebUI.openBrowser('')
WebUI.navigateToUrl('https://www.saucedemo.com')

// Input username
WebUI.setText(
    findTestObject('Object Repository/TC-01_Succed.Login/Page_Swag Labs/input_Username'),
    'standard_user'
)

// Input password (encrypted)
WebUI.setEncryptedText(
    findTestObject('Object Repository/TC-01_Succed.Login/Page_Swag Labs/input_Password'),
    'aGVsbG8='  // 'secret_sauce' dalam format enkripsi Katalon
)

// Klik tombol login
WebUI.click(
    findTestObject('Object Repository/TC-01_Succed.Login/Page_Swag Labs/input_login-button')
)

// Verifikasi berhasil login
WebUI.verifyElementText(
    findTestObject('Object Repository/TC-01_Succed.Login/Page_Swag Labs/div_Swag Labs'),
    'Swag Labs'
)

WebUI.closeBrowser()
```

---

## 4. Object Repository Detail

### Struktur Folder

```
Object Repository/
├── TC-01_Succed.Login/
│   └── Page_Swag Labs/
│       ├── input_Username.rs
│       ├── input_Password.rs
│       ├── input_login-button.rs
│       └── div_Swag Labs.rs
│
├── TC-02_Failed.Login1/
│   └── Page_Swag Labs/
│       ├── input_Username.rs
│       ├── input_Password.rs
│       ├── input_login-button.rs
│       └── h3_Epic sadface_ Username and password do not ma.rs
│
├── TC-03-Failed.Login2/
│   └── Page_Swag Labs/
│       ├── input_Password.rs
│       ├── input_login-button.rs
│       └── div_Epic sadface_ Username is required.rs
│
├── TC-04_Failed.Login3/
│   └── Page_Swag Labs/
│       ├── input_Username.rs
│       └── input_login-button.rs
│
├── TC-05_Failed.Login4/
│   └── Page_Swag Labs/
│       ├── input_login-button.rs
│       └── div_Epic sadface_ Username is required.rs
│
├── TC-06_Product Detail/
│   └── Page_Swag Labs/
│       ├── input_Username.rs
│       ├── input_Password.rs
│       ├── input_login-button.rs
│       └── img_Sauce Labs Backpack.rs
│
├── TC-07_Add Cart/
│   └── Page_Swag Labs/
│       ├── input_Username.rs
│       ├── input_Password.rs
│       ├── input_login-button.rs
│       ├── button_add-to-cart-sauce-labs-backpack.rs
│       ├── span_1.rs
│       └── div_Continue ShoppingCheckout.rs
│
└── TC-08_Remove Cart/
    └── Page_Swag Labs/
        ├── input_Username.rs
        ├── input_Password.rs
        ├── input_login-button.rs
        ├── button_add-to-cart-sauce-labs-backpack.rs
        ├── button_remove-sauce-labs-backpack.rs
        └── span_1.rs
```

### Definisi Elemen

Setiap file `.rs` berisi XML dengan definisi XPath/CSS selector:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<WebElementEntity>
   <description></description>
   <name>input_Username</name>
   <properties>
      <isSelected>true</isSelected>
      <matchCondition>equals</matchCondition>
      <name>xpath</name>
      <type>Main</type>
      <value>//*[@data-test = 'username']</value>
   </properties>
   <selectorCollection>
      <entry>
         <key>XPATH</key>
         <value>//*[@data-test = 'username']</value>
      </entry>
      <entry>
         <key>CSS</key>
         <value>#user-name</value>
      </entry>
   </selectorCollection>
   <selectorMethod>XPATH</selectorMethod>
</WebElementEntity>
```

### Mapping Elemen ke Selector

| Element Name | XPath | CSS Alternative |
|---|---|---|
| `input_Username` | `//*[@data-test='username']` | `#user-name` |
| `input_Password` | `//*[@data-test='password']` | `#password` |
| `input_login-button` | `//*[@data-test='login-button']` | `#login-button` |
| `button_add-to-cart-sauce-labs-backpack` | `//*[@data-test='add-to-cart-sauce-labs-backpack']` | - |
| `button_remove-sauce-labs-backpack` | `//*[@data-test='remove-sauce-labs-backpack']` | - |
| `img_Sauce Labs Backpack` | `//img[@alt='Sauce Labs Backpack']` | - |
| `span_1` | `//span[@class='shopping_cart_badge']` | `.shopping_cart_badge` |

---

## 5. Konfigurasi Project

### File Konfigurasi Utama

**`SauceDemo_Katalon.prj`** - Konfigurasi project Katalon:
```xml
<!-- Key settings -->
<projectType>WEBUI</projectType>
<edition>ENTERPRISE</edition>
<pageLoadTimeout>0</pageLoadTimeout>
```

**`Profiles/default.glbl`** - Global variables (kosong, tidak ada variabel custom):
```xml
<GlobalVariableEntities>
   <!-- Tidak ada variabel global yang terdefinisi -->
</GlobalVariableEntities>
```

**`build.gradle`** - Build configuration:
```gradle
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    // Semua dependency di-comment (tidak ada external dependency)
}
```

### Konfigurasi Test Suite

Setiap test suite dikonfigurasi dengan:
- **isReuseDriver**: `false` - Setiap test case menggunakan browser instance baru
- **pageLoadTimeout**: `10` detik
- **Browser**: Chrome (default)

---

## 6. Panduan Pengembangan

### Menambah Test Case Baru

1. **Buat Test Case baru** di Katalon Studio:
   - Klik kanan folder `Test Cases` > **New** > **Test Case**
   - Beri nama sesuai konvensi: `TC-XX_Nama Test Case`

2. **Buat Object Repository** untuk elemen baru:
   - Klik kanan folder `Object Repository` > **New** > **Test Object**
   - Atau gunakan fitur **Spy Web** untuk merekam elemen secara otomatis

3. **Tulis Script** di tab **Script** pada test case:
   ```groovy
   WebUI.openBrowser('')
   WebUI.navigateToUrl('https://www.saucedemo.com')
   // Tambahkan aksi pengujian di sini
   WebUI.closeBrowser()
   ```

4. **Daftarkan ke Test Suite** yang sesuai

### Konvensi Penamaan

| Komponen | Format | Contoh |
|----------|--------|--------|
| Test Case | `TC-XX_Deskripsi` | `TC-09_Succed Checkout Product` |
| Test Suite | `XX01-NamaModul` | `CO01-Checkout` |
| Object Repository Folder | `TC-XX_NamaModul` | `TC-09_Checkout` |
| Web Element | `tipe_identifier` | `button_checkout`, `input_firstName` |

### Rekomendasi Pengembangan Lanjutan

1. **Gunakan Global Variables** untuk menyimpan URL dan kredensial:
   ```groovy
   // Di Profiles/default.glbl, tambahkan:
   // BASE_URL = "https://www.saucedemo.com"
   // USERNAME = "standard_user"

   // Di script:
   WebUI.navigateToUrl(GlobalVariable.BASE_URL)
   ```

2. **Buat Custom Keywords** untuk aksi yang berulang (misalnya login):
   ```groovy
   // Keywords/LoginKeywords.groovy
   @Keyword
   def loginWithValidCredentials() {
       WebUI.setText(findTestObject('...input_Username'), GlobalVariable.USERNAME)
       WebUI.setEncryptedText(findTestObject('...input_Password'), GlobalVariable.PASSWORD)
       WebUI.click(findTestObject('...input_login-button'))
   }
   ```

3. **Data-Driven Testing** menggunakan Data Files untuk berbagai skenario

4. **Refaktor Object Repository** agar elemen yang sama tidak diduplikasi di setiap folder TC

---

## 7. Troubleshooting

### Masalah Umum dan Solusinya

#### Browser tidak terbuka
```
Solusi:
- Pastikan Chrome terinstall dan versinya kompatibel
- Update ChromeDriver di Katalon (Tools > Update WebDrivers > Chrome)
- Periksa: Window > Katalon Studio Preferences > Katalon > WebUI > Default Browser
```

#### Element tidak ditemukan (No Such Element)
```
Solusi:
- Buka Object Repository dan verifikasi XPath masih valid
- Gunakan Spy Web untuk memperbarui selector
- Cek apakah aplikasi SauceDemo ada perubahan UI
- Tambahkan delay: WebUI.delay(2) sebelum aksi pada elemen
```

#### Test gagal karena waktu loading
```
Solusi:
- Tambahkan WebUI.waitForElementVisible(obj, timeout) sebelum interaksi
- Contoh: WebUI.waitForElementVisible(findTestObject('...login-button'), 10)
```

#### Password encryption error
```
Solusi:
- Re-enkripsi password menggunakan: Help > Encrypt Text
- Atau gunakan WebUI.setText() untuk tujuan debugging (jangan di production)
```

#### Script tidak bisa di-run via Command Line
```
Solusi:
- Pastikan Katalon Runtime Engine (KRE) terinstall
- Verifikasi license KRE aktif
- Periksa path project di command katalonc
```

---

*Dokumentasi ini dibuat berdasarkan struktur project SauceDemo_Katalon pada tanggal 21 April 2026.*

