## Cashlez Transaction URL Scheme Documentation

### Overview

`cashlez://cdcp` adalah custom URL scheme yang digunakan untuk memulai proses pembayaran melalui aplikasi Cashlez. URL ini mendukung parameter untuk mengatur berbagai aspek transaksi, termasuk jumlah pembayaran, callback aplikasi, dan data tambahan.

---

### URL Format

```
cashlez://cdcp?launcher=true&amount=1000&print_receipt=true&callback=app2://result
```

---

### Parameters

Berikut adalah deskripsi parameter yang tersedia:

| **Parameter**       | **Type**     | **Required** | **Description**                                                                                         |
| ------------------- | ------------ | ------------ | --------------------------------------------------------------------------------------------------------|
| `launcher`          | Boolean      | Yes          | Menentukan apakah aplikasi anda sebagai launcher saja.                                                  |
| `amount`            | Integer      | Optional     | Jumlah pembayaran dalam satuan terkecil mata uang (contoh: 1000 untuk Rp 10.00).                        |
| `print_receipt`     | Boolean      | Optional     | Menentukan apakah receipt Cashlez akan auto print.                                                      |
| `callback`          | String (URL) | Optional     | URL callback untuk menerima hasil transaksi dari aplikasi Cashlez.                                      |
| `status` (Callback) | String       | Yes          | Status transaksi (`success`, `failed`, `cancelled`, `error`.). Ini merupakan bagian dari `callback`.    |
| `type` (Callback)   | String       | Yes          | Jenis transaksi (`cdcp`, `qris`, `prepaid`).                                                            |
| `data`              | JSON String  | Optional     | Informasi tambahan dalam format JSON yang dienkode sebagai URL (contoh: `{ "invoice": "120202" }`).     |

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
startActivityForResult(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp?launcher=true&amount=1000", 201)))
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

#### callback data json

##### success
```json
{
    "response_code": "0020",
    "is_debit_flag": false,
    "card_expiry_date": "2612",
    "card_holder_name": "JUMADI JANJAYA/          ",
    "batch_group": "BNI_CREDIT",
    "mid": "111101234012100",
    "tid": "20000120",
    "host_date": "0724",
    "pos_request_type": "1",
    "invoice_num": "000251",
    "base_amount": 10000,
    "approval_code": "288163",
    "midware_timestamp": "1690215056",
    "message": "SALE APPROVED.",
    "batch_num": "000009",
    "rrn": "000000000148",
    "masked_pan": "488950******1913",
    "print_receipt_merchant_name": "Toko Dinda",
    "bin_result": "VISA_BIN1",
    "host_time": "231056",
    "print_receipt_address_line_2": "Bali",
    "print_receipt_address_line_1": "Jalan Pemuda No.11",
    "status": "OK"
}
```
##### not approved
```json
{
  "response_code": "0P00",
  "is_debit_flag": true,
  "batch_group": "MTI",
  "midware_timestamp": "1737516071",
  "mid": "000071698997563",
  "bank_response_code_definition": "not define yet.",
  "message": "CDCP - SALE - FAILED.\nRESPONSE CODE FROM BANK NOT APPROVED.",
  "tid": "80623101",
  "acquirer_name": "MANDIRI - MTI",
  "masked_pan": "510481******8700",
  "bank_response_code": "C4",
  "trx_channel": "MTI_TRX_CHN_1",
  "trace_num": "010287",
  "status": "ERROR"
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

| Parameter         | Type            | Required     | Description                                                                                                      |
|--------------------|-----------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `invoice`          | String          | Optional     | Nomor invoice transaksi yang akan dibatalkan.                                                                   |
| `print_receipt`    | Boolean         | Optional     | Menentukan apakah receipt Cashlez akan auto print.                                                              |
| `callback`         | String (URL)    | Optional     | URL callback untuk menerima hasil pembatalan transaksi dari aplikasi Cashlez.                                   |
| `status` (Callback)| String          | Yes          | Status pembatalan transaksi (`success`, `failed`, `cancelled`, `error`.). Ini merupakan bagian dari `callback`. |
| `type` (Callback)  | String          | Yes          | Jenis transaksi (`cdcp`, `qris`,`cdcp.void`).                                                                   |
| `data`             | JSON String     | Optional     | Informasi tambahan dalam format JSON yang dienkode sebagai URL (contoh: `{ "invoice": "120202" }`).             |

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
startActivityForResult(Intent(Intent.ACTION_VIEW, Uri.parse("cashlez://cdcp.void?invoice=0012726", 201)))
```

#### **2. Callback dari Aplikasi Cashlez**
Jika pembatalan berhasil, aplikasi Cashlez akan mengirimkan callback ke URL berikut:
```
app2://result?status=success&type=cdcp.void&data=%7B%22invoce%22%3A%20%22120202%7D
```

---

### **Callback Parameters**
Aplikasi Anda harus menangani callback dengan parameter berikut:

| Parameter   | Type        | Description                                                                                |
|-------------|-------------|--------------------------------------------------------------------------------------------|
| `status`    | String      | Status pembatalan transaksi (`success`, `failed`, `cancelled`).                            |
| `type`      | String      | Jenis transaksi yang dilakukan (`cdcp`, `cdcp.void`).                                      |
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

#### callback data json

##### success
```json
{
    "response_code": "0031",
    "is_debit_flag": false,
    "card_expiry_date": "2612",
    "card_holder_name": "JUMADI JANJAYA/          ",
    "batch_group": "BNI_CREDIT",
    "trx_host_date": "0724",
    "mid": "111101234012100",
    "void_host_date": "0724",
    "tid": "20000120",
    "trx_host_time": "232540",
    "invoice_num": "000252",
    "base_amount": 1000,
    "approval_code": "948014",
    "void_host_time": "232907",
    "midware_timestamp": "1690216147",
    "message": "VOID APPROVED.",
    "batch_num": "000010",
    "rrn": "000000000149",
    "masked_pan": "488950******1913",
    "print_receipt_merchant_name": "Toko Dinda",
    "bin_result": "VISA_BIN1",
    "print_receipt_address_line_2": "Bali",
    "print_receipt_address_line_1": "Jalan Pemuda No.11",
    "status": "OK"
}
```

