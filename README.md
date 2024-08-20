![image](https://github.com/user-attachments/assets/f97d6142-98c4-422c-8e39-845f841d25da)

Kurulum videosu (eski kurulum fakat hiç bilmeyenler için yararlı olur izleyin) : [Link](https://youtu.be/SDNW5IrkLuc)

# Allora Hugging Face Worker Kurulum

# Daha önce kurduysanız sunucuyu sıfırlayıp baştan kurulum yapın.

Sunucuyu sıfırladıktan sonra terminalden bağlanırken sorun yaşarsanız ;

```
ssh-Keygen -R sunucuip
```
sunucuip yazan yere sunucu ip adresinizi yazıp enterlayın daha sonra sunucuya tekrar bağlanın.

# Sunucu Gereksinimleri

4 GB RAM

2 GB CPU

5 GB SSD

Ubuntu 22.04

Ben hetzner'in CX22 3.29€'luk sunucusunu aldım fazlasıyla yeterli olur.

Yeni kayıt olanlara digitalocean 2 aylık 200$ free hesap veriyor daha önce kullanmadıysanız bu [Link](https://t.co/5O8WuAtuHs)'e gidip kayıt olarak ücretsiz sunucu alabilirsiniz.

İlk başta bir tane keplr cüzdanı kuralım veya mevcutta varsa onu da kullanabilirsiniz cüzdanın 12 kelimelik tohumlarını bir kenarda bekletin kullanıcaz.(içerisinde varlık olan cüzdanınızı kullanmayın boş bir cüzdan olsun)

Daha sonra bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e tıklayıp keplr cüzdanınızı bağladıktan sonra şurdan allora adresimizi kopyalayalım.

![image](https://github.com/user-attachments/assets/038bf5f2-f5b2-4154-ac4a-560c339dec57)

Daha sonra bu [Link](https://faucet.testnet-1.testnet.allora.network/)'e tıklayıp cüzdan adresimizi yapıştırıp faucet alalım.

İsterseniz keplr cüzdanınıza bu [Link](https://explorer.testnet-1.testnet.allora.network/wallet/suggest)'e giderek allora test ağını ekleyebilirsiniz.

Şimdi sunucuda yapacağımız işlemlere geçelim.

Sunucuya bağlandıktan sonra sırasıyla ;

Aralarda y/n diye çıkarsa y yazıp enterlayın.

Sunucu güncelleyelim :

```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev curl git wget make jq build-essential pkg-config lsb-release libssl-dev libreadline-dev libffi-dev gcc screen unzip lz4 -y
```
Python yükleyelim :

```
sudo apt install python3
```
```
sudo apt install python3-pip
```

Docker yükleyelim :

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Docker-Compose yükleyelim :

```
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
```
```
curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
chmod +x /usr/local/bin/docker-compose
```
```
sudo groupadd docker
```
```
sudo usermod -aG docker $USER
```

Go yükleyelim : 

```
sudo rm -rf /usr/local/go
curl -L https://go.dev/dl/go1.22.4.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> $HOME/.bash_profile
source .bash_profile
```

Allora cüzdan kurulumu : 

```
git clone https://github.com/allora-network/allora-chain.git
```

(Burası biraz uzun sürebilir)

```
cd allora-chain && make all
```

Kullandığımız cüzdanı import edelim: (kodu girdikten sonra sizden cüzdanın tohum kelimelerini isteyecek daha sonra 2 kere şifre girmenizi isteyecek şifreyi girip devam edin.Cüzdana faucet aldığınızdan ve geldiğinden emin olun)

```
allorad keys add testkey --recover
```

Ana dizine dönüp hugging dosyasını çekelim :

```
cd
```
```
git clone https://github.com/allora-network/allora-huggingface-walkthrough
```
```
cd allora-huggingface-walkthrough
```
```
mkdir -p worker-data
```
```
chmod -R 777 worker-data
```
Config dosyasını kendinize göre düzenleyelin ve açılan boş ekrana yapıştırın ("cüzdan tohum kelimeleri" yazan yere kendi cüzdanınızın tohum kelimelerini yazın "" işaretlerini silmeden)

```
nano config.json
```

```
{
    "wallet": {
        "addressKeyName": "testkey",
        "addressRestoreMnemonic": "cüzdan tohum kelimeleri",
        "alloraHomeDir": "/root/.allorad",
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
            "loopSeconds": 1,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 2,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 3,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 3,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BTC"
            }
        },
        {
            "topicId": 4,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 2,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BTC"
            }
        },
        {
            "topicId": 5,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 4,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "SOL"
            }
        },
        {
            "topicId": 6,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "SOL"
            }
        },
        {
            "topicId": 7,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 2,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 8,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 3,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BNB"
            }
        },
        {
            "topicId": 9,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ARB"
            }
        }
        
    ]
}
```
Düzenledikten sonra kaydedip çıkmak için CTRL + X'e basın Y enter yapın.

Şimdi api giricez eğer api'niz yoksa bu [Link](https://www.coingecko.com/en/api)'e tıklayarak coingeckodan ücretsiz api alabilirsiniz.

Kayıt olduktan sonra "Get your api key now" yazan yere tıklayın biraz aşağı indiğinizde "Create Demo Account" yazısını görceksiniz ona tıklayın.

![image](https://github.com/user-attachments/assets/949de75b-7a1c-42f2-a102-7bb08b52bf77)

Buraları rastgele istediğiniz şekilde doldurabilirsiniz, Create Demo Account diyin daha sonra "+Add New Key" yazan yere tıklayın rastgele bir isim verin.

![image](https://github.com/user-attachments/assets/f7d14154-eed5-4e05-bc3b-1ae7db5d9106)

CG- ile başlayan sizin api keyiniz(CG- dahil hepsini kopyalayın)

Aşağıdaki kod ile app.py dosyasını açıyoruz

```
nano app.py
```
Klavyenin yön tuşlarıyla aşağı doğru inerek Your Coingecko API key yazan yere aldığınız api key'i yazın ( yalnızca "" tırnak işaretleri kalacak <> bu işaretleri de silin)

![image](https://github.com/user-attachments/assets/04c129e0-e2ba-418e-8497-c6da277188b8)

Düzenledikten sonra kaydedip çıkmak için CTRL + X'e basın Y enter yapın.

Şimdi çalıştıralım :

```
chmod +x init.config
./init.config
```
```
docker compose up --build -d
```
Logları kontrol etmek için :

```
docker logs -f worker
```
Bu şekilde çıktı aldıysanız tamamdır.Arada bir rpc error atabilir o normal.

![image](https://github.com/user-attachments/assets/2a0dea4c-6964-49fd-b304-e33ca00a31aa)



Evet kurulum bu kadar bu [Link](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6ImM1OTdmYjNlLWQ0ZGEtNGFmZi04MGJhLTVlOTAxYmZlZTBhNCJ9)'e gidip keplr cüzdanınızı bağlayarak puanınızı kontrol edebilirsiniz.Şuan puanlamada problem var 0 yazması normal puanlama düzeldiğinde geçmişe dönük olarak puanlar yansıyacak.
