---
sidebar_label: Kueri Rollup Anda
---

# 💬 Katakan "gm world!"

Sekarang, kita akan membuat blockchain kita mengatakan `gm world` dan untuk melakukannya kita perlu membuat perubahan berikut:

- Memodifikasi file buffer protokol
- Membuat fungsi kueri penjaga yang mengembalikan data
- Mendaftarkan fungsi kueri

File buffer protokol berisi panggilan proto RPC yang menentukan kueri Cosmos SDK dan penangan pesan, dan pesan proto yang mendefinisikan tipe Cosmos SDK. Panggilan RPC juga bertanggung jawab untuk mengekspos API HTTP.

Keeper diperlukan untuk setiap modul Cosmos SDK dan merupakan abstraksi untuk memodifikasi keadaan blockchain. Fungsi-fungsi Keeper memungkinkan Anda untuk menanyakan atau menulis ke state. Setelah Anda menambahkan kueri ke rantai Anda, Anda perlu mendaftarkan kueri tersebut. Anda hanya perlu mendaftarkan kueri sekali saja.

Alur kerja pengembang blockchain Cosmos yang khas terlihat seperti ini:

- Mulai dengan file proto untuk mendefinisikan Cosmos SDK [pesan](https://docs.cosmos.network/master/building-modules/msg-services.html)
- Tentukan dan daftarkan [queries](https://docs.cosmos.network/master/building-modules/query-services.html)
- Tentukan logika penangan pesan
- Terakhir, implementasikan logika dari kueri dan penangan pesan ini dalam fungsi keeper

## ✋ Buat kueri pertama Anda

**Untuk bagian tutorial ini, buka jendela terminal baru yang bukan jendela yang yang sama dengan tempat Anda memulai rantai.**

Di terminal baru Anda, `cd` ke dalam `gm` dan jalankan perintah ini untuk membuat `gm` kueri:

```bash
ignite scaffold query gm --response text
```

Tanggapan:

```bash
modify proto/gm/query.proto
modify x/gm/client/cli/query.go
create x/gm/client/cli/query_gm.go
create x/gm/keeper/grpc_query_gm.go

🎉 Membuat query `gm`.
```

Apa yang baru saja terjadi? `query` menerima nama kueri (`gm`), sebuah opsional opsional daftar parameter permintaan (kosong dalam tutorial ini), dan daftar opsional opsional daftar bidang respons yang dipisahkan koma dengan a `--response` flag (`text` dalam tutorial ini tutorial ini).

Navigate to the `proto/gm/query.proto` file, Anda akan melihat bahwa `Gm` RPC telah ditambahkan ke `Query` service:

<!-- markdownlint-disable MD010 -->
<!-- markdownlint-disable MD013 -->
```protobuf
service Query {
  rpc Params(QueryParamsRequest) returns (QueryParamsResponse) {
    option (google.api.http).get = "/gm/gm/params";
  }
    rpc Gm(QueryGmRequest) returns (QueryGmResponse) {
        option (google.api.http).get = "/gm/gm/gm";
    }
}
```
<!-- markdownlint-enable MD013 -->
<!-- markdownlint-enable MD010 -->

`Gm` RPC untuk `Query` service:

- bertanggung jawab untuk mengembalikan string `text`
- Menerima parameter permintaan (`QueryGmRequest`)
- Mengembalikan respons dengan tipe `QueryGmResponse`
- `option` mendefinisikan endpoint yang digunakan oleh gRPC untuk menghasilkan HTTP API

## 📨 Jenis permintaan kueri dan respons

Dalam file yang sama, kita akan menemukan:

- `QueryGmRequest` kosong karena tidak memerlukan parameter
- `QueryGmResponse` contains `text` ttopi dikembalikan dari rantai

```protobuf
message QueryGmRequest {
}

message QueryGmResponse {
  string text = 1;
}
```

## 👋 Fungsi penjaga Gm

`x/gm/keeper/grpc_query_gm.go` berisi berkas `Gm` fungsi penjaga yang menangani kueri dan mengembalikan data.

<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD010 -->
```go
func (k Keeper) Gm(goCtx context.Context, req *types.QueryGmRequest) (*types.QueryGmResponse, error) {
    if req == nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    ctx := sdk.UnwrapSDKContext(goCtx)
    _ = ctx
    return &types.QueryGmResponse{}, nil
}
```
<!-- markdownlint-enable MD010 -->
<!-- markdownlint-enable MD013 -->

Fungsi `Gm` melakukan tindakan berikut:

- Membuat pemeriksaan dasar pada permintaan dan melempar kesalahan jika `nil`
- Menyimpan konteks dalam variabel `ctx` yang berisi informasi tentang informasi tentang lingkungan permintaan
- Mengembalikan respons dengan tipe `QueryGmResponse`

Saat ini, responsnya kosong. Mari kita perbarui fungsi keeper.

Kami `query.proto` mendefinisikan bahwa respons menerima `text`. Gunakan teks Anda editor Anda untuk memodifikasi fungsi keeper di `x/gm/keeper/grpc_query_gm.go` .

<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD010 -->
```go
func (k Keeper) Gm(goCtx context.Context, req *types.QueryGmRequest) (*types.QueryGmResponse, error) {
    if req == nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    ctx := sdk.UnwrapSDKContext(goCtx)
    _ = ctx
    return &types.QueryGmResponse{Text: "gm world!"}, nil
}
```
<!-- markdownlint-enable MD010 -->
<!-- markdownlint-enable MD010 -->

## 🟢 Mulai Rollup Sovereign Anda

```bash
gmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"[http://localhost:26658](http://134.209.70.139:26658/)","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height 100783
```

`query` perintah juga telah diperancangkan `x/gm/client/cli/query_gm.go` yang mengimplementasikan CLI yang setara dengan query gm dan memasang perintah ini di `x/gm/client/cli/query.go`. Jalankan perintah berikut dan dapatkan yang berikut ini Tanggapan JSON:

```bash
gmd q gm gm
```

Tanggapan:

```bash
text: gm world!
```

![4.png](/img/gm/4.png)

Selamat 🎉 Anda telah berhasil membangun rollup pertama Anda dan menanyainya!
