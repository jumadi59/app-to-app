## Cashlez MiniATM URL Scheme Documentation

### **Overview**
Cashlez MiniATM menyediakan beberapa custom URL scheme untuk mempermudah berbagai transaksi seperti membuka aplikasi, cek saldo, transfer, tarik tunai, dan setor tunai.

---

### **1. Open App**

**URL Format**:
```
cashlez://miniatm.open
```

**Description**:
- URL ini digunakan untuk membuka aplikasi MiniATM Cashlez.

---

### **2. Cek Saldo**

**URL Format**:
```
cashlez://miniatm.checkbalance?launcher=true&saving_type=3000&print_receipt=true&callback=app2://result?type=miniatm.checkbalance
```

**Parameters**:
| **Parameter**       | **Type**        | **Required** | **Description**                                                                  |
|---------------------|-----------------|--------------|----------------------------------------------------------------------------------|
| `launcher`          | Boolean         | Yes          | Menentukan apakah aplikasi anda sebagai launcher saja.                           |
| `saving_type`       | String          | Optional     | Jenis tabungan yang akan dicek (saving: `1000`,current:`2000`).                  |
| `print_receipt`     | Boolean         | Optional     | Menentukan apakah struk akan dicetak. Nilai `true` untuk mencetak.               |
| `callback`          | String (URL)    | Optional     | URL callback untuk menerima hasil dari transaksi cek saldo.                      |

---

### **3. Transfer**

**URL Format**:
```
cashlez://miniatm.transfer?launcher=true&amount=1000&saving_type=3000&dest_bank_code=014&dest_bank_account=123456789000&print_receipt=true&callback=app2://result?type=miniatm.transfer
```

**Parameters**:
| **Parameter**       | **Type**        | **Required** | **Description**                                                                    |
|---------------------|-----------------|--------------|------------------------------------------------------------------------------------|
| `launcher`          | Boolean         | Yes          |  Menentukan apakah aplikasi anda sebagai launcher saja.                            |
| `amount`            | Integer         | Optional     | Jumlah uang yang akan ditransfer (dalam satuan terkecil mata uang, contoh: `1000`).|
| `saving_type`       | String          | Optional     | Jenis tabungan yang digunakan untuk transfer.                                      |
| `dest_bank_code`    | String          | Optional     | Kode bank tujuan transfer.                                                         |
| `dest_bank_account` | String          | Optional     | Nomor rekening tujuan transfer.                                                    |
| `print_receipt`     | Boolean         | Optional     | Menentukan apakah struk akan dicetak. Nilai `true` untuk mencetak.                 |
| `callback`          | String (URL)    | Optional     | URL callback untuk menerima hasil dari transaksi transfer.                         |

---

### **4. Tarik Tunai**

**URL Format**:
```
cashlez://miniatm.cashwithdrawal?launcher=true&amount=1000&saving_type=3000&print_receipt=true&callback=app2://result?type=miniatm.cashwithdrawal
```

**Parameters**:
| **Parameter**       | **Type**        | **Required** | **Description**                                                                  |
|---------------------|-----------------|--------------|----------------------------------------------------------------------------------|
| `launcher`          | Boolean         | Yes          | Menentukan apakah aplikasi anda sebagai launcher saja.                           |
| `amount`            | Integer         | Optional     | Jumlah uang yang akan ditarik (dalam satuan terkecil mata uang, contoh: `1000`). |
| `saving_type`       | String          | Optional     | Jenis tabungan yang digunakan untuk tarik tunai.                                 |
| `print_receipt`     | Boolean         | Optional     | Menentukan apakah struk akan dicetak. Nilai `true` untuk mencetak.               |
| `callback`          | String (URL)    | Optional     | URL callback untuk menerima hasil dari transaksi tarik tunai.                    |

---

### **5. Setor Tunai**

**URL Format**:
```
cashlez://miniatm.cashdeposit?launcher=true&amount=1000&saving_type=3000&print_receipt=true&callback=app2://result?type=miniatm.cashdeposit
```

**Parameters**:
| **Parameter**       | **Type**        | **Required** | **Description**                                                                  |
|---------------------|-----------------|--------------|----------------------------------------------------------------------------------|
| `launcher`          | Boolean         | Yes          | Menentukan apakah aplikasi anda sebagai launcher saja.                           |
| `amount`            | Integer         | Optional     | Jumlah uang yang akan disetor (dalam satuan terkecil mata uang, contoh: `1000`). |
| `saving_type`       | String          | Optional     | Jenis tabungan yang digunakan untuk setor tunai.                                 |
| `print_receipt`     | Boolean         | Optional     | Menentukan apakah struk akan dicetak. Nilai `true` untuk mencetak.               |
| `callback`          | String (URL)    | Optional     | URL callback untuk menerima hasil dari transaksi setor tunai.                    |

---

