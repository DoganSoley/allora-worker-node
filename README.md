![image](https://github.com/user-attachments/assets/f97d6142-98c4-422c-8e39-845f841d25da)

Kurulum videosu : 

# Sunucu Gereksinimleri

4 GB RAM

2 GB CPU

5 GB SSD

Ubuntu 22.04

Ben hetzner'in CX22 3.29€'luk sunucusunu aldım fazlasıyla yeterli olur.

Yeni kayıt olanlara digitalocean 2 aylık 200$ free hesap veriyor daha önce kullanmadıysanız bu [Link]([https://try.digitalocean.com/freetrialoffer/](https://t.co/5O8WuAtuHs)'e gidip kayıt olarak ücretsiz sunucu alabilirsiniz.

İlk başta bir tane keplr cüzdanı kuralım veya mevcutta varsa onu da kullanabilirsiniz cüzdanın 12 kelimelik tohumlarını bir kenarda bekletin kullanıcaz.

Daha sonra bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e tıklayıp keplr cüzdanınızı bağladıktan sonra şurdan allora adresimizi kopyalayalım.

![image](https://github.com/user-attachments/assets/038bf5f2-f5b2-4154-ac4a-560c339dec57)

Daha sonra bu [Link](https://faucet.testnet-1.testnet.allora.network/)'e tıklayıp cüzdan adresimizi yapıştırıp faucet alalım.

İsterseniz keplr cüzdanınıza bu [Link](https://explorer.testnet-1.testnet.allora.network/wallet/suggest)'e giderek allora test ağını ekleyebilirsiniz.

Şimdi sunucuda yapacağımız işlemlere geçelim.

Sunucuya bağlandıktan sonra sırasıyla ;

Güncelleme 

```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl git jq build-essential gcc unzip wget lz4 -y
```
Node çalıştırma 

```
wget https://raw.githubusercontent.com/dxzenith/allora-worker-node/main/allora.sh && chmod +x allora.sh && ./allora.sh
```
Yaklaşık 5-10dk kadar sürebilir yüklenmesi.


>Eğer "docker version" hatası alırsanız "Docker version hata düzeltme" kodlarını yazıp tekrar çalıştırma kodunu girin eğer hata vermezse bu adımı atlayın.

Docker version hata düzeltme 

```
sudo usermod -aG docker $USER
```
```
sudo reboot
```

Yükleme bittikten sonra bizden keplr cüzdan 12 kelimelik tohumları isteyecek onları girelim.

Bir şifre belirlemenizi isteyecek şifrenizi yazıp enterlayın tekrar onaylamanızı isteyecek tekrar yazıp enterlayın.

Daha sonra size "HEAD ID" verecek ve yazmanızı isteyecek onu yazalım.

Tekrar keplr cüzdan 12 kelimelik tohumları isteyecek tekrar girelim.

"Topic ID" isteyecek 1 yazın ve yüklenip çalışmasını bekleyin.

Son olarak v2 yükseltmesi yapalım.

V2 Yükseltme ;
```
wget -O testnetmigrator.sh https://raw.githubusercontent.com/casual1st/alloraworkersetup/main/testnetmigrator.sh && chmod +x testnetmigrator.sh && ./testnetmigrator.sh
```

Node doğru şekilde çalışıyor mu diye kontrol edelim.

Node durumu kontrol :
```
  curl --location 'http://localhost:6000/api/v1/functions/execute' \
--header 'Content-Type: application/json' \
--data '{
    "function_id": "bafybeigpiwl3o73zvvl6dxdqu7zqcub5mhg65jiky2xqb4rdhfmikswzqm",
    "method": "allora-inference-function.wasm",
    "parameters": null,
    "topic": "1",
    "config": {
        "env_vars": [
            {
                "name": "BLS_REQUEST_PATH",
                "value": "/api"
            },
            {
                "name": "ALLORA_ARG_PARAMS",
                "value": "ETH"
            }
        ],
        "number_of_nodes": -1,
        "timeout": 2
    }
}'
```

Bu şekilde bir çıktı alıyorsanız sorunsuz çalışıyor demektir.

![image](https://github.com/user-attachments/assets/82bd9d10-951a-4be4-89f4-8a3e21584d28)

Evet kurulum bu kadar bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e gidip keplr cüzdanınızı bağlayarak puanınızı kontrol edebilirsiniz.Eğer yeni kurduysanız 0 yazması normal haftalık olarak güncelleniyor.
