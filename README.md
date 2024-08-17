![image](https://github.com/user-attachments/assets/f97d6142-98c4-422c-8e39-845f841d25da)

Kurulum videosu : [Link](https://youtu.be/SDNW5IrkLuc)

# Kurulum güncel (daha önce kurduysanız güncelleme adımlarını aşağıda)

# Sunucu Gereksinimleri

4 GB RAM

2 GB CPU

5 GB SSD

Ubuntu 22.04

Ben hetzner'in CX22 3.29€'luk sunucusunu aldım fazlasıyla yeterli olur.

Yeni kayıt olanlara digitalocean 2 aylık 200$ free hesap veriyor daha önce kullanmadıysanız bu [Link](https://t.co/5O8WuAtuHs)'e gidip kayıt olarak ücretsiz sunucu alabilirsiniz.

İlk başta bir tane keplr cüzdanı kuralım veya mevcutta varsa onu da kullanabilirsiniz cüzdanın 12 kelimelik tohumlarını bir kenarda bekletin kullanıcaz.

Daha sonra bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e tıklayıp keplr cüzdanınızı bağladıktan sonra şurdan allora adresimizi kopyalayalım.

![image](https://github.com/user-attachments/assets/038bf5f2-f5b2-4154-ac4a-560c339dec57)

Daha sonra bu [Link](https://faucet.testnet-1.testnet.allora.network/)'e tıklayıp cüzdan adresimizi yapıştırıp faucet alalım.

İsterseniz keplr cüzdanınıza bu [Link](https://explorer.testnet-1.testnet.allora.network/wallet/suggest)'e giderek allora test ağını ekleyebilirsiniz.

Şimdi sunucuda yapacağımız işlemlere geçelim.

Sunucuya bağlandıktan sonra sırasıyla ;

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
y yazıp devam ediyoruz, yaklaşık 5-10dk kadar sürebilir yüklenmesi.


Yükleme bittikten sonra bizden keplr cüzdan 12 kelimelik tohumları isteyecek onları girelim ve  tekrar yüklemenin bitmesini bekleyelim.

# Buradan sonrası videoda farklı burdan devam edin.

![image](https://github.com/user-attachments/assets/e5973641-3bc6-4765-847e-cd5533a20136)

Bu ekran geldikten sonra CTRL + C yapıp logları kapatın ve aşağıdaki kodları girin sırasıyla.

# Config dosyasını düzenleme

```
cd basic-coin-prediction-node
```

```
rm -rf config.json
```
```
nano config.json
```

Burda boş bir ekran açılacak bu ekrana aşağıdaki kodu yapıştırın düzenlemek için klavyeden yön tuşlarını kullanın.(keplr 12 kelimelik tohum yazan kısma cüzdanınızın tohum kelimesini yazarak düzenleyin "" işaretlerini silmeden)
```
{
    "wallet": {
        "addressKeyName": "testkey",
        "addressRestoreMnemonic": "keplr 12 kelimelik tohum",
        "alloraHomeDir": "",
        "gas": "1000000",
        "gasAdjustment": 1.0,
        "nodeRpc": "https://sentries-rpc.testnet-1.testnet.allora.network/",
        "maxRetries": 1,
        "delay": 1,
        "submitTx": false
    },
    "worker": [
        {
            "topicId": 1,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 2,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 7,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        }
    ]
}
```

Daha sonra sırasıyla CTRL + X + Y enter yapın ve aşağıdaki kodları yazarak devam edin.

```
chmod +x init.config
./init.config
```
```
docker compose up -d --build
```

Yükleme bittikte sonra logları kontrol etmek için;
```
docker logs -f worker
```

Loglar bu şekilde akıyorsa çalışıyor demektir.(Daha uzun loglarda atıyor arada bir error atabiliyor biraz bekleyin aralarda txHash attığına emin olun)
![image](https://github.com/user-attachments/assets/c06d4c59-5fc4-4015-ada4-a187ed29afe0)


# Daha önce kurduysanız güncellemek için.(Yukarıdaki kurulumu yaptıysanız burayı yapmayın burası sadece eskiden kuranlar için)

Sunucuya bağlandıktan sonra ;

```
rm -rf allora.sh allora-chain/ basic-coin-prediction-node/
```
```
wget https://raw.githubusercontent.com/dxzenith/allora-worker-node/main/allora.sh && chmod +x allora.sh && ./allora.sh
```

Çıkan sorulara "y" yazarak enterlayın.

Tekrar cüzdan tohum kelimelerini isteyecek yapıştırıp devam edin.

yükleme bitip loglar akmaya başladıktan sonra CTRL + C yapın ve yukarıdaki "Config dosyasını düzenleme" kısmından aşağı doğru aynı işlemleri yapın.Loglar yukarıdaki fotoğraftaki gibi akıyorsa çalışıyor demektir.

Evet kurulum bu kadar bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e gidip keplr cüzdanınızı bağlayarak puanınızı kontrol edebilirsiniz.Şuan puanlamada problem var 0 yazması normal puanlama düzeldiğinde geçmişe dönük olarak puanlar yansıyacak.
