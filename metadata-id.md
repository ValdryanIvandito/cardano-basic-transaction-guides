# Pendahuluan

Dokumentasi ini menjelaskan cara membuat transaksi yang disertakan metadata di Cardano, ikuti langkah-langkah dibawah ini.

# Langkah-Langkah

## Langkah-1 Membuat Alamat Dompet (Optional)

Jika Anda belum membuat Alamat Dompet, ikuti [dokumentasi](https://github.com/ValdryanIvandito/cardano-basic-transaction-guide/blob/main/generate-wallet-address-id.md) berikut.

## Langkah-2 Inisiasi Jaringan Cardano (Opsional)

**_Petunjuk: Jika Anda sudah memilih jaringan, Anda dapat melewati langkah ini_**

```bash
network="testnet-magic 1"
```

_or_

```bash
network="testnet-magic 2"
```

_or_

```bash
network="mainnet"
```

**Berikut ini adalah tabel mengenai jaringan Cardano**
| cli Parameter | Network Name |
|---------|---------|
| testnet-magic 1 | Preprod |
| testnet-magic 2 | Preview |
| mainnet | Mainnet |

## Langkah-3 Inisiasi Parameter Input: Hash Transaksi (TxHash) dan Indeks Transaksi (TxIx) dari Alamat Dompet (Pengirim)

**_Petunjuk: Asumsi Anda telah memiliki Alamat Dompet_**

### Menampilkan Informasi Mengenai UTxO Alamat Dompet

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

**Contoh Hasil:**

```bash
                           TxHash                                 TxIx        Amount
--------------------------------------------------------------------------------------
62c0ce8d6e0b584e9e263e3ba076f53c23095ebd0a9198305819cfa5ecef8e81     0        1000000000 lovelace + TxOutDatumNone
```

### Inisiasi TxHash dan TxIx

```bash
utxo="COPY TX-HASH DISINI#COPY TX-IX DISINI"
```

**_Catatan: TxHash dan TxIx dibatasi antara '#'_**

## Langkah-4 Inisiasi Parameter Output: Alamat Penerima dan Jumlah ADA yang Akan Dikirim

```bash
recipientAddress="COPY ALAMAT PENERIMA DISINI"
amount="JUMLAH DALAM LOVELACE"
```

**_Catatan: 1₳ = 1,000,000 Lovelace_**

## Langkah-5 Membuat Metadata JSON

**_Petunjuk: Untuk membuat metadata JSON, Anda dapat menggunakan Vim atau Nano_**

### Menggunakan Vim

```bash
vim metadata.json
```

**Instruksi:**

1. Tekan karakter 'i' pada keyboard untuk memasuki mode insert
2. Copy dan paste contoh metada dibawah ini:

```JSON
{
  "674": {
    "msg": [
      "Cardano Developer Community Indonesia"
    ]
  }
}
```

3. Tekan 'Esc' pada keyboard untuk keluar dari mode insert
4. Ketik :wq untuk menyimpan dan keluar dari Vim

### Menggunakan Nano

```bash
nano metadata.json
```

**Instruksi:**

1. Copy dan paste contoh metada dibawah ini:

```JSON
{
  "674": {
    "msg": [
      "Cardano Developer Community Indonesia"
    ]
  }
}
```

2. Tekan CTRL + x
3. Kemudian muncul pesan seperti ini -> (If prompted with "Save modified buffer?"), tekan Y, lalu tekan Enter untuk menyimpan

## Langkah-6 Membuat Transaksi

```bash
cardano-cli transaction build \
--babbage-era \
--$network \
--tx-in $utxo \
--tx-out $recipientAddress+$amount \
--metadata-json-file metadata.json \
--change-address $myAddress \
--out-file transaction.raw
```

## Langkah-7 Menandatangani Transaksi

```bash
cardano-cli transaction sign \
--$network \
--tx-body-file transaction.raw \
--signing-key-file payment.skey \
--out-file transaction.signed
```

## Langkah-8 Mengirim Transaksi

```bash
cardano-cli transaction submit \
--$network \
--tx-file transaction.signed
```

**_Petunjuk: Anda dapat melacak transaksi menggunakan penjelajah blockchain, seperti Cardano Explorer atau CardanoScan. Salin tautan di bawah ini._**

Preprod:

```bash
https://preprod.cardanoscan.io/transaction/COPY-TX-HASH-DISINI
```

Preview:

```bash
https://preview.cardanoscan.io/transaction/COPY-TX-HASH-DISINI
```

Mainnet:

```bash
https://cardanoscan.io/transaction/COPY-TX-HASH-DISINI
```

# Demo

Berikut adalah video yang direkam oleh Komunitas Developer Cardano Indonesia di mana saya menjelaskan langkah-langkah di atas. Tonton video yang direkam pada timestamp **_1:27:27_** di [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ) berikut ini.

# Referensi

[Gimbalabs PPBL2023 Module 203.1: Transaction Metadata](https://plutuspbl.io/modules/203/2031)

[Cardano Developer Portal: Metadata Transaction Guide](https://developers.cardano.org/docs/transaction-metadata/how-to-create-a-metadata-transaction-cli/)

[CIP-0010: Transaction Metadata Label Registry](https://github.com/cardano-foundation/CIPs/blob/868ae58447c953cc6115b61064af6d5ad30edd87/CIP-0010/README.md)
