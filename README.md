````markdown
# 🛡️ Mawari Guardian Node Kurulum Rehberi (TestNet)

Bu rehber, **Mawari Network TestNet** üzerinde **Guardian Node** kurmak isteyenler için adım adım hazırlanmıştır.  
MainNet sürümünde komutlar ve yapılandırmalar değişebilir.

````

## 📦 Gereksinimler

| Bileşen | Minimum Gereksinim |
|--------|--------------------|
| 🖥️ İşletim Sistemi | Ubuntu 20.04 / 22.04 |
| 🧠 CPU | 2 vCPU |
| 💾 RAM | 4 GB |
| 📂 Disk | 20 GB SSD |
| 🔐 Cüzdan | MetaMask |



## 🪙 1. Test Token ve Guardian NFT Alma

Kuruluma başlamadan önce aşağıdakileri tamamlayın:

- MetaMask cüzdanınızı açın ve **TestNet ağına geçin**
- https://hub.testnet.mawari.net adresinden test token alın
- https://testnet.mawari.net adresinden **3 adet Guardian NFT** mint edin



## 🔌 2. Sunucuya Bağlanma

Terminal üzerinden VPS’inize bağlanın:

```bash
ssh root@sunucu_ip_adresi
````

---

## 🐳 3. Docker Kurulumu

Guardian Node, Docker konteyneri içinde çalışır. Eğer Docker yüklü değilse:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
```

> ⚠️ **Not:** Komut sonrası oturumu kapatıp tekrar bağlanın:

```bash
exit
ssh root@sunucu_ip_adresi
```

---

## 🚀 4. Guardian Node Kurulumu

Çalışma dizinimizi oluşturup node’u başlatıyoruz.
`0xCUZDAN_ADRESI` kısmını kendi MetaMask adresinizle değiştirin:

```bash
mkdir -p ~/mawari

docker run -d \
  --name mawari-node \
  --restart unless-stopped \
  -v ~/mawari:/app/cache \
  -e OWNERS_ALLOWLIST=0xCUZDAN_ADRESI \
  us-east4-docker.pkg.dev/mawarinetwork-dev/mwr-net-d-car-uses4-public-docker-registry-e62e/mawari-node:latest
```

---

## 🔑 5. Burner Wallet Adresini Alma

Node açıldığında otomatik olarak bir **burner wallet** oluşturur.
30 saniye bekleyip adresi öğrenin:

```bash
sleep 30
docker logs mawari-node 2>&1 | grep "Using burner wallet"
```

---

## 🔁 6. Delegation İşlemi

Guardian NFT’lerinizi burner cüzdanınıza devredin:

1. Burner wallet adresine **0.5 - 1 test token** gönderin
2. [https://app.testnet.mawari.net](https://app.testnet.mawari.net) adresine gidin
3. **3 Guardian NFT’yi** burner wallet adresinize delegate edin

---

## 🧪 Faydalı Komutlar

| İşlem                       | Komut                               |                |          |
| --------------------------- | ----------------------------------- | -------------- | -------- |
| 📊 Node durumunu kontrol et | `docker ps`                         |                |          |
| 📜 Son logları görüntüle    | `docker logs --tail 50 mawari-node` |                |          |
| ❤️ Heartbeat kontrolü       | `docker logs mawari-node 2>&1       | grep heartbeat | tail -5` |
| 🔄 Node'u yeniden başlat    | `docker restart mawari-node`        |                |          |
| ⏹️ Node'u durdur            | `docker stop mawari-node`           |                |          |
| ❌ Node'u sil                | `docker rm -f mawari-node`          |                |          |

---


---

## 🛠️ Sorun Giderme

**🔧 Node çalışmıyor:**

```bash
docker logs mawari-node
```

**🌐 Port sorunu yaşarsanız:**

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```


---

## 🤝 Destek ve Topluluk

* 💬 [Mawari Discord](https://discord.gg/mawari)
* 🐦 [@MawariNetwork](https://twitter.com/MawariNetwork)

---
