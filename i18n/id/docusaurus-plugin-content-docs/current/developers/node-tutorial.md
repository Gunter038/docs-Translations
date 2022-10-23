---
sidebar_label: Tata cara Node
---

# Mendapatkan dan Mengirim Transaksi dengan Node Celestia
<!-- markdownlint-enable MD013 -->

Dalam ata cara ini, kita akan menjelaskan bagaimana menggunakan API node Celestia untuk mengirim dan mendapatkan kembali pesan dari layer Ketersedian Data dengan ID namaspacenya.

Tata cara ini berasumsi kamu bekerja di environment Linux.

> Untuk melihat tata cara video dalam mengatur Node Light Celestia, klik [disini](./light-node-video.md)

## Spesifikasi Hardware

Berikut minimal spesifikasi hardware yang direkomendasikan untuk menjalankan node light:

- Memori: 2 GB RAM
- CPU: Single Core
- Disk: Penyimpanan 5 GB SSD
- Bandwidth: 56 Kbps untuk Unduh/56 Kbps untuk Unggah

## Atur Dependensi

Pertama, pastikan untuk memperbarui dan meningkatkan versi OS:

```sh
# Jika kamu menggunakan APT package manager
sudo apt update && sudo apt upgrade -y

# Jika kamu menggunakan YUM package manager
sudo yum update
```

Itu ialah paket tambahan yang diperlukan untuk mengeksekusi banyak tugas seperti penggunduhan file, penyatuan, dan pemantauan node:

<!-- markdownlint-disable MD013 -->
```sh
# Jika kamu menggunakan APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Jika kamu menggunakan YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

### Install Golang

Aplikasi-Celestia dan node-celestia ditulis dalam [Golang](https://go.dev/) jadi kita harus install Golang untuk membangun dan menjalankannya.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Jadi kita butuh menambahkan direktori `/usr/local/go/bin` ke `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Untuk memeriksa jika Go yang terinstall berjalan dengan benar:

```sh
go version
```

Versi yang terinstall harusnya keluar seperti:

```sh
go version go1.18.2 linux/amd64
```

## Node Celestia

### Install Node Celestia

Install binary node-celestia dengan menjalankan perintah berikut:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

Verifikasi binary berjalan dan periksa versi dengan perintah versi celestia:

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Instansiasi Node Light Celestia

Sekarang, mari instansiasi node Light Celestia:

> Catatan: RPC endpoint terungkap di semua tipe Node Celestia seperti Light, Bridge dan Full node.

```sh
celestia light init
```

### Hubungkan ke Enpoint Inti Publik

Mari sekarang jalankan node Light Celestia dengan koneksi GRPC contoh Endpoint inti publik.

> Catatan: Kamu juga didorong untuk menemukan komunitas yang menjalankan endpoint API dan beberapa disana di Discord. Salah satunya digunakan untuk bertujuan demontrasi. Kamu dapat menemukan daftar RPC endpoint [disini](/nodes/arabica-devnet.md#rpc-endpoints)

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

> CATATAN: Port bawaan `--core.grpc.port` 9090, jadi jika kamu tidak menentukannya di baris perintah, itu akan menjadi port bawaan. Kamu dapat menggunakan bendera untuk menentukan port lainnya jika kamu ingin.

Sebagai contoh, perintahmu ke RPC endpoint mungkin terlihat seperti ini:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Kunci dan Dompet

Kamu dapat membuat kuncimu untuk nodemu dengan menjalankan perintah berikut:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

Kamu dapat memulai node lightmu dengan kunci yang dibuat dengan menjalankan perintah diatas:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name> 
```
<!-- markdownlint-enable MD013 -->

Ketika kamu memulai Node Light, kunci dompet akan dihasilkan untukmu. Kamu akan butuh mendanai alamat dengan token Devnet Arabica untuk membayar transaksi PayForData.

Kamu dapat menemukan alamat dengan menjalankan perintah berikut di direktori `celestia-node`:

```sh
./cel-key list --node.type light --keyring-backend test
```

Jika kamu ingin mendanai dompetmu dengan token testnet, beralihlah ke saluran Discord Celestia `#arabica-faucet`.

Kamu dapat meminta dana untuk alamat dompetmu menggunakan perintah berikut di Discord:

```console
$request <Wallet-Address>
```

Yang mana `<Wallet-Address>` ialah alamat `celestia1******` yang dihasilkan ketika kamu membuat dompet.

Dengan dompetmu yang telah terdanai, kamu dapat berpindah ke langkat berikutnya.

## API Call Node

Buka window terminal lain untuk memulai proses query API. `celestia-node` mengungkap RPC endpoint pada port bawaan `26658`.

### Saldo

Sekarang, mari query node kita untuk saldo akun bawaan ini (yang mana akun berasosiasi dengan kunci `developer` yang kita hasilkan diawal):

```sh
curl -X GET http://127.0.0.1:26658/balance
```

Ini akan keluar sebagai berikut:

```json
{
   "denom":"utia",
   "amount":"999995000000000"
}
```

Ini menunjukkanmu saldo di dompet.

### Dapatkan Header Block

Sekarang, mari mendapatkan informasi header block.

Disini kita akan mendapatkan header dari Block 1:

```sh
curl -X GET http://127.0.0.1:26658/header/1
```

Ini akan keluar seperti ini:

```json
{
   "header":{
      "version":{
         "block":11
      },
      "chain_id":"devnet-2",
      "height":1,
      "time":"2021-11-23T02:24:05.965728875Z",
      "last_block_id":{
         "hash":"",
         "parts":{
            "total":0,
            "hash":""
         }
      },
      "last_commit_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "data_hash":"7B578B351B1B0BBD70BB350019EBC964C44A140A37EF715B552A7F8F315ACD19",
      "validators_hash":"7F4EA93A134DEDBDA6A1FDD30D05760DD98A2B5FBA95DB3EFFFE7FCE4B361855",
      "next_validators_hash":"7F4EA93A134DEDBDA6A1FDD30D05760DD98A2B5FBA95DB3EFFFE7FCE4B361855",
      "consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
      "app_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "last_results_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "evidence_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "proposer_address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E"
   },
   "commit":{
      "height":1,
      "round":1,
      "block_id":{
         "hash":"4632277C441CA6155C4374AC56048CF4CFE3CBB2476E07A548644435980D5E17",
         "parts":{
            "total":1,
            "hash":"BE1B789969938DB061B7723BE45C8A7B4B27192339A8E0A4E216775B2CB58D97"
         }
      },
      "signatures":[
         {
            "block_id_flag":2,
            "validator_address":"03F1044A6DF782189C7061FF89146B3D33608F17",
            "timestamp":"2021-11-23T11:53:56.123958759Z",
            "signature":"q/XF7Nc2ThcQgWfqi6LYOUEqLcU+sgVPY1nB2CLWdjRo80WwI6xy6QaYx1B0lmcKAkWR0YMxbc7vJzKF70qwBA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E",
            "timestamp":"2021-11-23T11:53:56.036027188Z",
            "signature":"0bbxeviCvgLlyIF47JoY1CMN2e/MhHtFzhdgV0zCM+P3qeO/rkh+0TSxoRVUB1MDvMCoA8hyffCw3amxympMAA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"31711F367349D1BD619BD0A39568A69614B8A048",
            "timestamp":"2021-11-23T11:53:56.135943844Z",
            "signature":"gT23rZ8+XcG5rQ9QS+uh+Wn5eAiJDVy8bh23Fb8hevnHsuO1er2892MXAohaLRF6TTWs/C6dItYph4B/k3b6DQ=="
         },
         {
            "block_id_flag":2,
            "validator_address":"5A253EC2A9AB20AFD48C7BED2AFCA53F5C80BCA6",
            "timestamp":"2021-11-23T11:53:56.081698742Z",
            "signature":"nMngIlJ7PPPtu0N20YwKhWt/H3/JrEKJC3rnS5KG/8J3IppTacuwjGDUqIuHpRlrD0MmWa1mlY+6+ulbytt2AA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"79BEB39F4B396F9278DA03F1C97F9BE3B10B12D3",
            "timestamp":"2021-11-23T11:53:56.037896319Z",
            "signature":"wPAL/sK4YXSU7iRXl00GDLvi4IItVlkY2zRkxIUeV9VA3Tq8Tke6wGE0N6pXKmtMcKUMoyjm03RWHv4mrtj1BA=="
         },
         {
            "block_id_flag":1,
            "validator_address":"",
            "timestamp":"0001-01-01T00:00:00Z",
            "signature":null
         },
         {
            "block_id_flag":1,
            "validator_address":"",
            "timestamp":"0001-01-01T00:00:00Z",
            "signature":null
         },
         {
            "block_id_flag":2,
            "validator_address":"D345D62BBD18C301B843DF7C65F10E57AB17BD98",
            "timestamp":"2021-11-23T11:53:56.123499237Z",
            "signature":"puj5Epw3yPGjSsJk6aQI74S2prgL7+cuiEpCxXYzQxOi0kNIqh8UMZLYf+AVHDQNJXehSmrAK6+VsIt9NF0DDg=="
         },
         {
            "block_id_flag":2,
            "validator_address":"DEC2642E786A941511A401090D21621E7F08A36D",
            "timestamp":"2021-11-23T11:53:56.123136439Z",
            "signature":"J/lnFqARXj42Lfx5MGY0FO/wug+AyQRxTnQp1u1HyczvV+0hXXuk06Uosi61jQKgJG6JBJF2VidqA41/uKMEDw=="
         }
      ]
   },
   "validator_set":{
      "validators":[
         {
            "address":"03F1044A6DF782189C7061FF89146B3D33608F17",
            "pub_key":"sMcFgSIzlD77eZYgV7H4akyxoHCPc2oIQW05qWEB6b4=",
            "voting_power":5000,
            "proposer_priority":-40000
         },
         {
            "address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E",
            "pub_key":"WdqZ8hoyc1HxZCJfQrAGKm2fFJZFg7PngPNGkA1RWXc=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"31711F367349D1BD619BD0A39568A69614B8A048",
            "pub_key":"pvwSRksq3ekXIiYK7IzjQJ870BxLqEma8zRr9n9VnXI=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"5A253EC2A9AB20AFD48C7BED2AFCA53F5C80BCA6",
            "pub_key":"RnmnTlKoKxNoh2TpohBDP3cKlx4ATiPOCvQFk/6xpUU=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"79BEB39F4B396F9278DA03F1C97F9BE3B10B12D3",
            "pub_key":"oh/N+GOIennBOAa/gPNCso1mDlqaHQNn7Op/X8opbeY=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"7F1105B7B219481810C49730AECB1A83036BCA3A",
            "pub_key":"Ow/AHP/Q3guPGymUKpvhnwae+QoCOpGztpVnP179IG8=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"87265CC17922E01497F40B701EC9F05373B83467",
            "pub_key":"MNi0Z+uNF5X1Bxj988IDXVl0BKUcLs7LItoMnX6dbg4=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"D345D62BBD18C301B843DF7C65F10E57AB17BD98",
            "pub_key":"4g3hhdyU4IIgWW/4sR0nax8bsC/M/fDbt1N8s/QanF8=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"DEC2642E786A941511A401090D21621E7F08A36D",
            "pub_key":"b+Vv6Lcp0bhIjOQncr+OYBHixCvU5+k34y4RqyvpluE=",
            "voting_power":5000,
            "proposer_priority":5000
         }
      ],
      "proposer":{
         "address":"03F1044A6DF782189C7061FF89146B3D33608F17",
         "pub_key":"sMcFgSIzlD77eZYgV7H4akyxoHCPc2oIQW05qWEB6b4=",
         "voting_power":5000,
         "proposer_priority":-40000
      }
   },
   "dah":{
      "row_roots":[
         "//////////7//////////uyLCVMJmAItYqbOqgHXm3OwHsq1xQiAX1kZV2Tgcobm",
         "/////////////////////ykyWNfDJZfigziZC5BN5L00KKuoyDPduwynDywauskL"
      ],
      "column_roots":[
         "//////////7//////////uyLCVMJmAItYqbOqgHXm3OwHsq1xQiAX1kZV2Tgcobm",
         "/////////////////////ykyWNfDJZfigziZC5BN5L00KKuoyDPduwynDywauskL"
      ]
   }
}
```

### Kirim Transaksi PFD

Sebagai contoh, kita akan memngirim transaksi PayForData ke node endpoint `/submit_pfd`.

Beberapa hal yang dipertimbangkan:

- PFD ialah Pesan PayForData.
- Endpoint juga mengambil `namespace_id` dan nilai `data`.
- Namespace ID should be 8 bytes.
- Data is in hex-encoded bytes of the raw message.
- `gas_limit` is the limit of gas to use for the transaction

We use the following `namespace_id` of `0000010000000100` and the `data` value of `f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5`.

You can generate your own `namespace_id` and data values using this useful Golang Playground we created [here](https://go.dev/play/p/7ltvaj8lhRl).

We run the following:

```sh
curl -X POST -d '{"namespace_id": "0c204d39600fddd3",
  "data": "f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5",
  "gas_limit": 70000}' http://localhost:26658/submit_pfd
```

We get the following output:

```json
{
   "height":2452,
   "txhash":"04A79AF9DA62FDB41ACD7D82EB0B9004AE4E4ED603B280A65816560B4F38A999",
   "data":"12200A1E2F7061796D656E742E4D7367506179466F7244617461526573706F6E7365",
   "raw_log":"[{\"msg_index\":0,\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/payment.MsgPayForData\"}]},{\"type\":\"payfordata\",\"attributes\":[{\"key\":\"signer\",\"value\":\"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw\"},{\"key\":\"size\",\"value\":\"27\"}]}]}]",
   "logs":[
      {
         "msg_index":0,
         "events":[
            {
               "type":"message",
               "attributes":[
                  {
                     "key":"action",
                     "value":"/payment.MsgPayForData"
                  }
               ]
            },
            {
               "type":"payfordata",
               "attributes":[
                  {
                     "key":"signer",
                     "value":"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw"
                  },
                  {
                     "key":"size",
                     "value":"27"
                  }
               ]
            }
         ]
      }
   ],
   "events":[
      {
         "type":"coin_spent",
         "attributes":[
            {
               "key":"spender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"coin_received",
         "attributes":[
            {
               "key":"receiver",
               "value":"celestia17xpfvakm2amg962yls6f84z3kell8c5lpnjs3s",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"transfer",
         "attributes":[
            {
               "key":"recipient",
               "value":"celestia17xpfvakm2amg962yls6f84z3kell8c5lpnjs3s",
               "index":true
            },
            {
               "key":"sender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"message",
         "attributes":[
            {
               "key":"sender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws/267",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"JMNihnKS/MtYJDprqEFGJuXh16tVADsDDxXaFFpvv2te57btl4LbiRzwRRiN2rvwkJ2zlAApu2ImT22MZBi5+A==",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia13zx48t96zauht0kpcn0kcfykc9wn8fehzcp9wq/1024",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"mIZIjbzN0/RQAlQN7TDWzqtey3vVBPe7IO3+IIDhJstIH8QU9vsHfl0Rql9qWMZQG4dM+77w9WmUcnCeS7edfw==",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia1h36gnnwzneu0csqzn2waph5y983hf3dkaznlgz/0",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"sfy+XyP7iWU+V9q3zEIOWxbGihvhzUKRLNVeXP+a+5oRefIA/Pyqfm13A5NU9I27hhfvpqo9vhXW1waRgcI9OA==",
               "index":true
            }
         ]
      },
      {
         "type":"message",
         "attributes":[
            {
               "key":"action",
               "value":"/payment.MsgPayForData",
               "index":true
            }
         ]
      },
      {
         "type":"payfordata",
         "attributes":[
            {
               "key":"signer",
               "value":"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw",
               "index":true
            },
            {
               "key":"size",
               "value":"27",
               "index":true
            }
         ]
      }
   ]
}
```

If you notice from the above output, it returns a `height` of `2452` which we will use for the next command.

Note: To learn more about status response codes, please navigate to [cosmos' code explanation](https://github.com/cosmos/cosmos-sdk/blob/main/types/errors/errors.go)

#### Troubleshooting

If you encounter an error like:

<!-- markdownlint-disable MD013 -->
```console
$ curl -X POST -d '{"namespace_id": "c14da9d459dc57f5", "data": "4f7a3f1aadd83255b8410fef4860c0cd2eba82e24a", "gas_limit": 60000}'  localhost:26658/submit_pfd
"rpc error: code = NotFound desc = account celestia1krkle0n547u0znz3unnln8paft2dq4z3rznv86 not found"
```
<!-- markdownlint-enable MD013 -->

It is possible that the account you are trying to submit a PayForData from doesn't have testnet tokens yet. Ensure the testnet faucet has funded your account with tokens and then try again.

### Get Namespaced Shares by Block Height

After submitting your PFD transaction, upon success, the node will return the block height for which the PFD transaction was included. You can then use that block height and the namespace ID with which you submitted your PFD transaction to get your message shares returned to you. In this example, the block height we got was 589 which we will use for the following command.

```sh
curl -X GET \
  http://localhost:26658/namespaced_shares/0c204d39600fddd3/height/2452
```

Will generate the following output:

```json
{
   "shares":[
      "DCBNOWAP3dMb8fIMqAB+kQo7+LLmHaDya8oH73hxem6lQWX1AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=="
   ],
   "height":2452
}
```

The output here is base64-encoded.
