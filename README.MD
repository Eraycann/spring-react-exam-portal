# ExamSystem

<p align="center">
  <video controls width="720" src="docs/sunum.mp4">
    Tarayıcınız video etiketini desteklemiyor. Videoyu indirmek için <a href="sunum.mov">sunum.mov</a> bağlantısını kullanın.
  </video>
</p>

ExamSystem, eğitim kurumları için uçtan uca sınav yönetimi sağlayan bir platformdur. Kurs yönetimi, sınav planlama, soru havuzu, öğrenci kayıtları ve gönderim takibi gibi süreci tek bir çözümde birleştirir.

## Ne çözülüyor?

Sistem, eğitimde şu sorunları adresler:

- kurs ve sınav verilerinin farklı yerlerde saklanması
- öğretmenlerin sınav ve soru sürecini elle yönetmek zorunda kalması
- öğrencilerin sınavlara erişim ve kaydının dağınık olması
- güvenli kimlik doğrulama ve yetkilendirme ihtiyacı

ExamSystem, bu ihtiyaçları aşağıdaki şekilde ele alır:

- kurs, ders, sınav ve soru yönetimini tek bir merkezde toplar
- rollere göre öğretmen, öğrenci ve yönetici deneyimini ayrı kılar
- JWT ve Google OAuth2 ile güvenli oturum açma sağlar
- frontend ile backend arasında token yenileme ve izin kontrolü sunar

## Neden tercih edilmeli?

- açık iş akışı: `Dashboard`, `My Courses`, `Take Exam`, `Create Course`, `Exam Submissions`
- güçlü güvenlik: JWT tabanlı API yetkilendirmesi ve sosyal giriş desteği
- modüler yapı: backend mantığı `auth`, `course`, `exam`, `student`, `teacher`, `topic` şeklinde ayrılmış
- hızlı frontend: React + Vite ile modern yönetim paneli deneyimi
- ayrı geliştirme: backend ve frontend birbirinden bağımsız çalışır, kolay test edilir

## Mimari

- `ExamSystem/` — Spring Boot backend
- `ExamSystemFrontend/` — React + Vite frontend

Backend, REST API ve kimlik doğrulama kontrollerini yönetir. Frontend ise kullanıcı arayüzünü sağlar ve `http://localhost:8080/api` üzerinden backend ile iletişim kurar.

## Kullanıcı odaklı özellikler

### Öğretmenler
- kurs oluşturma ve düzenleme
- sınav programlama
- soru havuzu yönetimi
- gönderimleri inceleme
- öğrenci kayıtlarını takip etme

### Öğrenciler
- kayıtlı kurslara erişim
- sınavlara katılma
- sınav cevaplarını gönderme
- geçmiş gönderimleri görme

### Ortak özellikler
- kullanıcı kaydı ve oturum açma
- Google OAuth2 ile giriş
- JWT tabanlı token yenileme
- rol bazlı erişim kontrolü

## Üretime hazır nokta

Aşağıdaki adımlar projeyi geliştirme ortamından üretime taşımak için temel yol haritasını verir.

### 1. Veritabanı

- PostgreSQL üzerinde `ExamSystem` veritabanı oluşturun.
- `ExamSystem/src/main/resources/application.properties` içindeki bağlantı bilgilerini güncelleyin.
- Üretimde `spring.jpa.hibernate.ddl-auto=validate` veya `none` kullanın; `update` yalnızca geliştirme için uygundur.

### 2. Güvenlik

- `application.security.jwt.secret-key` için güçlü ve gizli bir anahtar kullanın.
- Google OAuth2 için:
  - `GOOGLE_CLIENT_ID`
  - `GOOGLE_CLIENT_SECRET`
- `app.oauth2.redirectUri` değeri frontend’deki callback URL ile eşleşmelidir.

### 3. Backend paketleme

```powershell
cd ExamSystem
./mvnw clean package
```

Üretim için oluşturulan JAR, `target/` klasöründe yer alır.

### 4. Frontend paketleme

```powershell
cd ExamSystemFrontend
npm install
npm run build
```

Üretim yapısı `dist/` klasörüne çıkar.

## Çalıştırma

### Geliştirme

```powershell
cd ExamSystem
./mvnw spring-boot:run
```

```powershell
cd ExamSystemFrontend
npm run dev
```

### Üretim

- backend: `java -jar ExamSystem/target/ExamSystem-0.0.1-SNAPSHOT.jar`
- frontend: üretilen `dist/` klasörünü bir web sunucusuna (Nginx, Apache, CDN) dağıtın

> Frontend varsayılan olarak `http://localhost:5173` üzerinde çalışır. API istekleri `http://localhost:8080/api` adresine yönlendirilmelidir.

## Konfigürasyon

### Backend ayarları

`ExamSystem/src/main/resources/application.properties` önemli alanlar:

- `spring.datasource.url`
- `spring.datasource.username`
- `spring.datasource.password`
- `spring.jpa.hibernate.ddl-auto`
- `application.security.jwt.secret-key`
- `application.security.jwt.expiration`
- `application.security.jwt.refresh-token.expiration`
- `spring.security.oauth2.client.registration.google.client-id`
- `spring.security.oauth2.client.registration.google.client-secret`
- `app.oauth2.redirectUri`

### Frontend ayarları

`ExamSystemFrontend/src/utils/apiService.js` içinde API tabanlı:

- temel URL: `http://localhost:8080/api`
- otomatik token yenileme (401 durumunda)
- 403 yetki hatası yönetimi

## Proje yapısı

### Backend

- `ExamSystem/src/main/java/org/kafka/examsystem`
  - `auth`
  - `course`
  - `exam`
  - `exam_question`
  - `exam_submission`
  - `student`
  - `teacher`
  - `topic`
  - `user`

### Frontend

- `ExamSystemFrontend/src`
  - `pages`
  - `components`
  - `contexts`
  - `services`
  - `utils`

## Genişletme önerileri

- notlandırma ve raporlama özellikleri ekleme
- canlı sınav/zamanlayıcı desteği ekleme
- kurs bazlı bildirim sistemi kurma
- frontend ve backend entegrasyonunu tek bir dağıtım hattına taşıma

## Temel avantajlar

- düzenli işlem akışı ve rol bazlı erişim
- hem öğretmen hem öğrenci hem yönetici için tek platform
- modern frontend ve sağlam backend altyapısı
- üretime hazırlık için ayrı paketleme adımları
