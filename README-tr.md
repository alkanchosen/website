# Kubernetes Dökümantasyonu

[![Netlify Status](https://api.netlify.com/api/v1/badges/be93b718-a6df-402a-b4a4-855ba186c97d/deploy-status)](https://app.netlify.com/sites/kubernetes-io-master-staging/deploys) [![GitHub release](https://img.shields.io/github/release/kubernetes/website.svg)](https://github.com/kubernetes/website/releases/latest)

Bu depo [Kubernetes sitesini ve dökümantasyonunu](https://kubernetes.io/) kurmak için gerekli olan verileri içerir. Katkı sağlamak istediğiniz için minnettarız!

# Deponun kullanımı

Siteyi yerel olarak Hugo (geliştirilmiş versiyon) ile veya bir container'de çalıştırabilirsiniz. Biz container kullanmanızı öneriyoruz çünkü canlı site ile dağıtım tutarlılığı sağlıyor.

## Gereklilikler

Bu depoyu kullanabilmeniz için aşağıdakilerin kurulu olması gerekmektedir:

- [npm](https://www.npmjs.com/)
- [Go](https://golang.org/)
- [Hugo (Geliştirilmiş versiyon)](https://gohugo.io/)
- [Docker](https://www.docker.com/) gibi bir container.

Başlamadan önce bütün bağımlılıkları kurun. Depoyu klonlayın ve klasöre gidin:

```
git clone https://github.com/kubernetes/website.git
cd website
```

Kubernetes sitesi [Docsy Hugo temasını](https://github.com/google/docsy#readme) kullanır. Siteyi container'de çalıştırmayı planlıyorsanız bile alt modülü ve diğer bağımlılıkları çekmenizi şiddetle öneriyoruz:

```
# pull in the Docsy submodule
git submodule update --init --recursive --depth 1
```

## Siteyi container'da çalıştırmak

Siteyi container'da çalıştırmak için aşağıdaki kodları çalıştırın ve container imajını oluşturun:

```
make container-image
make container-serve
```

Siteyi görüntülemek için tarayıcınızda http://localhost:1313 adresine girin. Kaynak kodlarda değişiklik yapıldıkça Hugo siteyi güncelleyecek ve tarayıcınızı yenileyecek.

## Siteyi Hugo kullanarak yerelde çalıştırmak

[`netlify.toml`](netlify.toml#L10) dosyasındaki `HUGO_VERSION` değişkeninde belirtilen Hugo geliştirilmiş versiyonu kurduğunuzdan emin olun.

Siteyi yerel olarak kurmak ve çalıştırmak için, şu kodu çalıştırın:

```bash
# install dependencies
npm ci
make serve
```

Bu kod 1313 portunda yerel bir Hugo sunucusu başlatacak. Siteyi görüntülemek için tarayıcınızda http://localhost:1313 adresini açın. Kaynak kodlarda değişiklik yapıldıkça Hugo siteyi güncelleyecek ve tarayıcınızı yenileyecek.

## Hata giderme
### error: failed to transform resource: TOCSS: failed to transform "scss/main.scss" (text/x-scss): this feature is not available in your current Hugo version

Hugo teknik nedenlerden dolayı iki farklı sürümle dağıtılmaktadır. Şu anki websitesi sadece **Hugo Geliştirilmiş** versiyonda çalışmaktadır. [Sürüm sayfasında](https://github.com/gohugoio/hugo/releases) isminde `extended` olan arşivlere bakın. Onaylamak için, `hugo version` komutunu çalıştırın ve `extended` kelimesini arayın.

### MacOS'da çok fazla açık dosyadan dolayı hata giderme

Eğer MacOS'da `make serve` komutunu çalıştırıp aşağıdaki hatayı alıyorsanız:

```
ERROR 2020/08/01 19:09:18 Error: listen tcp 127.0.0.1:1313: socket: too many open files
make: *** [serve] Error 1
```

Açık dosyalar için mevcut limiti kontrol etmeyi deneyin:

`launchctl limit maxfiles`

Sonra aşağıdaki kodu çalıştırın (https://gist.github.com/tombigel/d503800a282fcadbee14b537735d202c adresinden uyarlanmıştır):

```
#!/bin/sh

# These are the original gist links, linking to my gists now.
# curl -O https://gist.githubusercontent.com/a2ikm/761c2ab02b7b3935679e55af5d81786a/raw/ab644cb92f216c019a2f032bbf25e258b01d87f9/limit.maxfiles.plist
# curl -O https://gist.githubusercontent.com/a2ikm/761c2ab02b7b3935679e55af5d81786a/raw/ab644cb92f216c019a2f032bbf25e258b01d87f9/limit.maxproc.plist

curl -O https://gist.githubusercontent.com/tombigel/d503800a282fcadbee14b537735d202c/raw/ed73cacf82906fdde59976a0c8248cce8b44f906/limit.maxfiles.plist
curl -O https://gist.githubusercontent.com/tombigel/d503800a282fcadbee14b537735d202c/raw/ed73cacf82906fdde59976a0c8248cce8b44f906/limit.maxproc.plist

sudo mv limit.maxfiles.plist /Library/LaunchDaemons
sudo mv limit.maxproc.plist /Library/LaunchDaemons

sudo chown root:wheel /Library/LaunchDaemons/limit.maxfiles.plist
sudo chown root:wheel /Library/LaunchDaemons/limit.maxproc.plist

sudo launchctl load -w /Library/LaunchDaemons/limit.maxfiles.plist
```

Bu Catalina ve Mojave MacOS'ta çalışır.

# SIG Docs'a katılın

SIG Docs Kubernetes topluluğu ve toplantıları hakkında bilgi sahibi olmak için [topluluk sayfasına](https://github.com/kubernetes/community/tree/master/sig-docs#meetings) gidin.

Bu projenin sorumlularına şu adreslerle ulaşabilirsiniz:

- [Slack](https://kubernetes.slack.com/messages/sig-docs), [Slack için bir davetiye alın](https://slack.k8s.io/)
- [Mail Listesi](https://groups.google.com/forum/#!forum/kubernetes-sig-docs)

# Dökümantasyona katkı sağlamak

Bu deponun GitHub hesabınızda bir kopyasını yaratmak için ekranın sağ üstünde bulunan **Fork** butonuna tıklayabilirsiniz. Bu kopya *fork* olarak adlandırılır. Yapmak istediğiniz değişiklikleri fork'ta yapın ve değişiklikleri bize göndermek için hazır olduğunuzda fork'a gidip yeni bir *pull request* yaratın.

*Pull request* yaratıldıktan sonra bir Kubernetes görevlisi açık ve yapılabilir bir geri bildirim gönderecek. Pull request'in sahibi olarak **Kubernetes görevlisi tarafından size gönderilen geri bildirime göre pull request'inizi güncellemek sizin sorumluluğunuzdadır.**

Ayrıca birden fazla Kubernetes görevlisi tarafından geri bildirim alabilir veya size geri bildirim vermesi için atanan Kubernetes görevlisinden farklı bir görevli tarafından geri bildirim alabilirsiniz.

Bazı durumlarda görevlilerden birisi teknik bir gözden geçirme için isteğinizi Kubernetes teknik görevlilerine gönderebilir. Görevliler size vaktinde geri bildirim göndermek için ellerinden gelenin en iyisini yapacaklar fakat duruma göre cevap süresi değişebilir.

Kubernetes dökümantasyonuna katkı ile alakalı daha fazla bilgi için aşağıdaki bağlantılara bakabilirsiniz:

* [Contribute to Kubernetes docs](https://kubernetes.io/docs/contribute/)
* [Page Content Types](https://kubernetes.io/docs/contribute/style/page-content-types/)
* [Documentation Style Guide](https://kubernetes.io/docs/contribute/style/style-guide/)
* [Localizing Kubernetes Documentation](https://kubernetes.io/docs/contribute/localization/)

# Yerelleştirme `README.md`'leri

| Language  | Language |
|---|---|
|[Chinese](README-zh.md)|[Korean](README-ko.md)|
|[French](README-fr.md)|[Polish](README-pl.md)|
|[German](README-de.md)|[Portuguese](README-pt.md)|
|[Hindi](README-hi.md)|[Russian](README-ru.md)|
|[Indonesian](README-id.md)|[Spanish](README-es.md)|
|[Italian](README-it.md)|[Ukrainian](README-uk.md)|
|[Japanese](README-ja.md)|[Vietnamese](README-vi.md)|
|[Turkish](README-tr.md)|

# Davranış kodu

Kubernetes topluluğuna katılımınız [CNCF Davranış Kodu](https://github.com/cncf/foundation/blob/master/code-of-conduct.md) tarafından korunur.

# Teşekkür ederiz!

Kubernetes topluluk katılımı için can atıyor. Sitemize ve dökümantasyonumuza yaptığınız katkılardan dolayı minnettarız!


