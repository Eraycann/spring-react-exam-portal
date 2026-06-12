# ExamSystem

**ExamSystem**, öğrenci sınav yönetimi için geliştirilmiş bir tam yığın uygulamadır. Spring Boot tabanlı backend ile React + Vite frontend bileşeni içerir. Proje, kurs yönetimi, sınav oluşturma, soru yönetimi, öğrenci kaydı, yanıt gönderimi ve sınav sonuç takibi gibi işlevlere sahiptir.

## Proje Yapısı

- `ExamSystem/` - Spring Boot backend uygulaması
- `ExamSystemFrontend/` - React + Vite frontend uygulaması

## Öne Çıkan Özellikler

- Kullanıcı kimlik doğrulaması ve yetkilendirme
- JWT tabanlı token yönetimi ve refresh token desteği
- Google OAuth2 ile sosyal giriş desteği
- PostgreSQL veri tabanı bağlantısı
- Sınav, soru, konu, kurs, öğrenci, öğretmen ve ilgili kayıt yönetimi
- Öğrenci kayıtları, sınav gönderimleri ve cevap inceleme
- React tabanlı modern yönetici ve öğrenci arayüzü

## Teknolojiler

### Backend

- Java 21
- Spring Boot 3.3.1
- Spring Security
- Spring Data JPA
- Spring Boot Validation
- Spring Boot Mail
- Springdoc OpenAPI
- MapStruct
- JJWT
- PostgreSQL
- Lombok

### Frontend

- React 19
- Vite
- React Router DOM
- Material UI
- Tailwind CSS
- ESLint

## Gereksinimler

- Java 21
- Maven
- Node.js ve npm
- PostgreSQL

## Backend Kurulum ve Çalıştırma

1. Veri tabanını hazırlayın
   - PostgreSQL içinde `ExamSystem` adında bir veri tabanı oluşturun.
   - `application.properties` dosyasındaki bağlantı bilgilerini ihtiyacınıza göre güncelleyin.

2. Ortam değişkenlerini ayarlayın
   - Google OAuth2 için:
     - `GOOGLE_CLIENT_ID`
     - `GOOGLE_CLIENT_SECRET`

3. Backend servisini çalıştırın

```powershell
cd ExamSystem
./mvnw spring-boot:run
```

> Varsayılan olarak backend `http://localhost:8080` adresinde çalışır.

## Frontend Kurulum ve Çalıştırma

1. Bağımlılıkları yükleyin

```powershell
cd ExamSystemFrontend
npm install
```

2. Frontend uygulamasını çalıştırın

```powershell
npm run dev
```

> Frontend, `http://localhost:5173` üzerinde çalışır ve API isteklerini `http://localhost:8080/api` üzerinden backend'e yönlendirir.

## Yapılandırma Detayları

### Backend - `ExamSystem/src/main/resources/application.properties`

- `spring.datasource.url` - PostgreSQL bağlantı adresi
- `spring.datasource.username` - veri tabanı kullanıcı adı
- `spring.datasource.password` - veri tabanı parola
- `spring.jpa.hibernate.ddl-auto=update` - veri tabanı şemasını otomatik günceller
- `application.security.jwt.secret-key` - JWT imzalama anahtarı
- `application.security.jwt.expiration` - erişim token süresi (ms)
- `application.security.jwt.refresh-token.expiration` - refresh token süresi (ms)
- Google OAuth2 client ayarları için `GOOGLE_CLIENT_ID` ve `GOOGLE_CLIENT_SECRET` kullanılır
- `app.oauth2.redirectUri` - OAuth2 callback URL

### Frontend - `ExamSystemFrontend/src/utils/apiService.js`

- API temel URL: `http://localhost:8080/api`
- `accessToken` ve `refreshToken` tabanlı otomatik yenileme
- 401 durumunda refresh token ile token yenileme
- 403 durumunda yetkisiz erişim kontrolü

## Proje Modülleri

### Backend modülleri

- `auth` - kimlik doğrulama, JWT ve OAuth2 işlemleri
- `user` - platform kullanıcıları
- `student` / `teacher` - öğrenci ve öğretmen yönetimi
- `course` / `course_student` - kurs yönetimi ve öğrenci kurs kaydı
- `exam` / `exam_question` / `exam_submission` - sınav, soru ve gönderim yönetimi
- `topic` - konu yönetimi
- `student_answer` - sınav cevapları
- `common` - ortak yardımcı bileşenler ve DTO'lar

### Frontend bileşenleri

- `components/` - modaller, sayfa bileşenleri ve kullanıcı etkileşimleri
- `contexts/` - kimlik doğrulama bağlamı
- `hooks/` - özel kancalar
- `pages/` - uygulama sayfaları
- `services/` - kimlik doğrulama servisleri
- `utils/` - API servisleri, sabitler ve hata işleme

## Kullanım Akışı

1. Kullanıcı `Login` veya `Register` işlemi ile giriş yapar.
2. Frontend, JWT token ile istekleri backend'e gönderir.
3. Backend kimlik doğrulama, yetkilendirme ve kurs/sınav işlemlerini yönetir.
4. Öğrenciler sınavlara katılır, soruları yanıtlar ve sınav gönderir.
5. Öğretmenler sınavları oluşturur, soruları düzenler ve gönderimleri inceleyebilir.

## Derleme ve Üretim İçin

### Backend paketleme

```powershell
cd ExamSystem
./mvnw clean package
```

### Frontend üretim yapısı

```powershell
cd ExamSystemFrontend
npm run build
```

## İpuçları

- `spring.jpa.hibernate.ddl-auto=update` geliştirme ortamı için uygundur. Üretim ortamında dikkatli kullanılmalıdır.
- Google OAuth2 için `redirectUri` ayarının frontend URL ile uyumlu olduğundan emin olun.
- API tabanlı frontend geliştirme yaparken backend çalışır durumda olmalıdır.

## İletişim

Bu README temel kurulum, geliştirme ve yapılandırma bilgilerini içerir. Projeyi genişletirken backend `src/main/java/org/kafka/examsystem` ve frontend `src` klasörlerini inceleyebilirsiniz.
