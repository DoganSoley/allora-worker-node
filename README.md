![image](https://github.com/user-attachments/assets/f97d6142-98c4-422c-8e39-845f841d25da)

Kurulum videosu : [Link](https://youtu.be/SDNW5IrkLuc)

# Kurulum videoya göre biraz değişti aşağıya buradan sonrası videodan farklı diye fotoğraflı anlatarak devam edicem.

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

# Buradan sonrası videoda farklı burdan devam edin.

Böyle bir ekran verip container_id girmenizi isteyecek kırmızıyla işaretlediğim kendi node-worker container_id'nizi kopyalayıp yapıştırıp enterlayın.

![allora yeni](https://github.com/user-attachments/assets/1d67314a-beb1-4098-bd1d-fe5a06f8b501)

Daha sonra böyle bir ekran gelecek CTRL + C yapın ve alttaki V2 Yükseltme kodunu girin.

![allora2](https://github.com/user-attachments/assets/1011c3aa-ecf0-43d0-9568-feeb7fbf76ef)

V2 Yükseltme ;
```
wget -O testnetmigrator.sh https://raw.githubusercontent.com/casual1st/alloraworkersetup/main/testnetmigrator.sh && chmod +x testnetmigrator.sh && ./testnetmigrator.sh
```
Burda faucet almanız gerektiğini hatırlatıyor aldığımız için herhangi bir tuşa basıp devam ediyoruz.(Faucet aldığınızdan emin olun)

![testnet2](https://github.com/user-attachments/assets/1e56aeb2-4b1c-4a32-9736-fc9e972e5d70)

Yükleme işlemi bittikten sonra ;

```
docker ps -a
```
Yazıyoruz ve çıkan ekranda yine kırmızıyla işaretlediğim node-worker ile biten container_id kopyalayıp

![Ekran Alıntısı](https://github.com/user-attachments/assets/584540ad-d2e2-4e98-b119-e6562cf250de)

```
docker logs -f containerid
```
containerid yazan yeri kendi container_id'niz ile değiştirin ve enterlayın.(ilk kurduğumuz ile aynı değil bu yeni containerid)

Loglar bu şekildeyse bir problem yoktur.Loglardan çıkmak için CTRL + C yapabilirsiniz.(bu şekilde değilse sabit bekliyorsa da 20-30dk sonra tekrar kontrol edin)

![allora 3](https://github.com/user-attachments/assets/d68f4288-7478-4be8-b5ec-e5fe9dff75f6)

Son bir kontrol daha yapalım.(Sadece buna baksanız da olur fakat bakmışken ikisine de bakın garanti olsun :) )

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

![image](https://github.com/user-attachments/assets/61eaf05e-2d4e-4052-bace-2277dd390482)


Evet kurulum bu kadar bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e gidip keplr cüzdanınızı bağlayarak puanınızı kontrol edebilirsiniz.Eğer yeni kurduysanız 0 yazması normal haftalık olarak güncelleniyor.
