## Cashlez Trancaction URL Scheme Documentation

### Overview

`cashlez://cdcp` adalah custom URL scheme yang digunakan untuk memulai proses pembayaran melalui aplikasi Cashlez. URL ini mendukung parameter untuk mengatur berbagai aspek transaksi, termasuk jumlah pembayaran, callback aplikasi, dan data tambahan.

---

### URL Format

```
cashlez://cdcp?launcher=true&amount=1000&callback=app2://result
```

---

### Parameters

Berikut adalah deskripsi parameter yang tersedia:

| **Parameter**       | **Type**     | **Required** | **Description**                                                                                     |
| ------------------- | ------------ | ------------ | --------------------------------------------------------------------------------------------------- |
| `launcher`          | Boolean      | Yes          | Menentukan apakah aplikasi Cashlez akan diluncurkan. Nilai harus `true`.                            |
| `amount`            | Integer      | Optional     | Jumlah pembayaran dalam satuan terkecil mata uang (contoh: 1000 untuk Rp 10.00).                    |
| `callback`          | String (URL) | Optional     | URL callback untuk menerima hasil transaksi dari aplikasi Cashlez.                                  |
| `status` (Callback) | String       | Yes          | Status transaksi (`success`, `failed`, `cancelled`, `error`.). Ini merupakan bagian dari `callback`.    |
| `type` (Callback)   | String       | Yes          | Jenis transaksi (`cdcp`, `qris`).                                                       |
| `data`              | JSON String  | Optional     | Informasi tambahan dalam format JSON yang dienkode sebagai URL (contoh: `{ "invoice": "120202" }`). |

---

### Example Usage

#### **1. Memulai Pembayaran**

URL untuk memulai pembayaran sejumlah Rp 10.00:

```
cashlez://cdcp?launcher=true&amount=1000&callback=app2://result
```

#### **Kotlin Example**
```kotlin
startActivity(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp?launcher=true&amount=1000&callback=app2://result")))
// or
startActivityForResult(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp?launcher=true&amount=1000&callback=app2://result", 201)))
```

#### **2. Callback dari Aplikasi Cashlez**

Jika pembayaran berhasil, aplikasi Cashlez akan mengirimkan callback ke URL berikut:

```
app2://result?status=success&type=cdcp&data=%7B%22invoce%22%3A%20%22120202%7D
```

---

### Callback Parameters

Aplikasi Anda harus menangani callback dengan parameter berikut:

| **Parameter** | **Type**    | **Description**                                                                            |
| ------------- | ----------- | ------------------------------------------------------------------------------------------ |
| `status`      | String      | Status transaksi (`success`, `failed`, `cancelled`).                                       |
| `type`        | String      | Jenis transaksi yang dilakukan (`cdcp`).                                                   |
| `data`        | JSON String | Informasi tambahan tentang transaksi dalam format JSON. Contoh: `{ "invoice": "120202" }`. |

---

### Handling the Callback in Your App

Berikut adalah contoh kode untuk menangani callback di aplikasi:

#### **Kotlin Example**

```kotlin

// from callback
override fun onNewIntent(intent: Intent?) {
    super.onNewIntent(intent)
    intent?.data?.let { uri ->
        val status = uri.getQueryParameter("status")
        val type = uri.getQueryParameter("type")
        val data = uri.getQueryParameter("data")

        if (status == "success" && type == "cdcp") {
            // Decode JSON data
            val decodedData = URLDecoder.decode(data, "UTF-8")
            val jsonObject = JSONObject(decodedData)
            val invoice = jsonObject.optString("invoice_num")

            // Handle success
            Toast.makeText(this, "Payment successful for invoice: $invoice", Toast.LENGTH_LONG).show()
        } else {
            // Handle other statuses
            Toast.makeText(this, "Transaction failed or cancelled", Toast.LENGTH_LONG).show()
        }
    }
}
// from ActivityResult
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        
        val status = data?.getStringExtra("status")
        val type = data?.getStringExtra("type")
        val data = data?.getStringExtra("data")

        if (status == "success" && type == "cdcp") {
            val jsonObject = JSONObject(data)
            val invoice = jsonObject.optString("invoice_num")

            // Handle success
            Toast.makeText(this, "Payment successful for invoice: $invoice", Toast.LENGTH_LONG).show()
        } else {
            // Handle other statuses
            Toast.makeText(this, "Transaction failed or cancelled", Toast.LENGTH_LONG).show()
        }
    }
```


## Cashlez Void URL Scheme Documentation

### **Overview**
`cashlez://cdcp.void` adalah custom URL scheme yang digunakan untuk membatalkan (void) transaksi melalui aplikasi Cashlez. URL ini mendukung parameter untuk mengatur informasi transaksi yang akan dibatalkan, termasuk nomor invoice dan callback aplikasi.

---

### **URL Format**
```
cashlez://cdcp.void?invoice=0012726&callback=app2://result
```

---

### **Parameters**
Berikut adalah deskripsi parameter yang tersedia:

| Parameter         | Type            | Required     | Description                                                                                              |
|--------------------|-----------------|--------------|----------------------------------------------------------------------------------------------------------|
| `invoice`          | String          | Optional     | Nomor invoice transaksi yang akan dibatalkan.                                                           |
| `callback`         | String (URL)    | Optional     | URL callback untuk menerima hasil pembatalan transaksi dari aplikasi Cashlez.                            |
| `status` (Callback)| String          | Yes          | Status pembatalan transaksi (`success`, `failed`, `cancelled`, `error`.). Ini merupakan bagian dari `callback`. |
| `type` (Callback)  | String          | Yes          | Jenis transaksi (`cdcp`, `qris`,`cdcp.void`).                                                            |
| `data`             | JSON String     | Optional     | Informasi tambahan dalam format JSON yang dienkode sebagai URL (contoh: `{ "invoice": "120202" }`).      |

---

### **Example Usage**
#### **1. Memulai Void Transaksi**
URL untuk membatalkan transaksi dengan nomor invoice `0012726`:
```
cashlez://cdcp.void?invoice=0012726&callback=app2://result
```

#### **Kotlin Example**
```kotlin
startActivity(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp.void?invoice=0012726&callback=app2://result")))
// or
startActivityForResult(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp.void?invoice=0012726&callback=app2://result", 201)))
```

#### **2. Callback dari Aplikasi Cashlez**
Jika pembatalan berhasil, aplikasi Cashlez akan mengirimkan callback ke URL berikut:
```
app2://result?status=success&type=cdcp.void&data=%7B%22invoce%22%3A%20%22120202%7D
```

---

### **Callback Parameters**
Aplikasi Anda harus menangani callback dengan parameter berikut:

| Parameter   | Type        | Description                                                                 |
|-------------|-------------|-----------------------------------------------------------------------------|
| `status`    | String      | Status pembatalan transaksi (`success`, `failed`, `cancelled`).            |
| `type`      | String      | Jenis transaksi yang dilakukan (`cdcp`, `cdcp.void`).                                   |
| `data`      | JSON String | Informasi tambahan tentang transaksi dalam format JSON. Contoh: `{ "invoice": "120202" }`. |

---

### **Handling the Callback in Your App**
Berikut adalah contoh kode untuk menangani callback di aplikasi:

#### **Kotlin Example**
```kotlin

// from callback
override fun onNewIntent(intent: Intent?) {
    super.onNewIntent(intent)
    intent?.data?.let { uri ->
        val status = uri.getQueryParameter("status")
        val type = uri.getQueryParameter("type")
        val data = uri.getQueryParameter("data")

        if (status == "success" && type == "cdcp.void") {
            // Decode JSON data
            val decodedData = URLDecoder.decode(data, "UTF-8")
            val jsonObject = JSONObject(decodedData)
            val invoice = jsonObject.optString("invoice")

            // Handle success
            Toast.makeText(this, "Void successful for invoice: $invoice", Toast.LENGTH_LONG).show()
        } else {
            // Handle other statuses
            Toast.makeText(this, "Void transaction failed or cancelled", Toast.LENGTH_LONG).show()
        }
    }
}

// from ActivityResult
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        val status = data?.getStringExtra("status")
        val type = data?.getStringExtra("type")
        val data = data?.getStringExtra("data")

        if (status == "success" && type == "cdcp") {
            val jsonObject = JSONObject(data)
            val invoice = jsonObject.optString("invoice_num")

            // Handle success
            Toast.makeText(this, "Payment successful for invoice: $invoice", Toast.LENGTH_LONG).show()
        } else {
            // Handle other statuses
            Toast.makeText(this, "Transaction failed or cancelled", Toast.LENGTH_LONG).show()
        }
    }
```

