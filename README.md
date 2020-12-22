# Veyosis_API_DOCS
Veyosis.com API Dökümanları
---
swagger: '2.0'
info:
  title: VEYOSIS API
  version: v1.0
  description: VEYOSİS API'yi kullanmak veya görüşlerinizi bizimle paylaşmak isterseniz
    lütfen bizimle info@veyosis.com adresi üzerinden iletişime geçiniz.
  termsOfService: https://veyosis.com/privacy_and_security
  x-logo:
    url: https://veyosis.com/assets/img/logo-api-docs.png
  contact:
    url: https://veyosis.com
    email: info@veyosis.com
  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html
host: api.veyosis.com
tags:
- name: GİRİŞ
  description: ''
- name: KİMLİK DOĞRULAMA
  description: "## API Kodu Oluşturma\n\n VEYOSIS API ile işlem gerçekleştirmek istiyorsanız
    öncelikle bir `API KOD`unuzun olması gerekmektedir. \n\nEğer API kodunuz yoksa;\n-
    https://panel.veyosis.com adresi üzerinde kaydınız olması gerekmektedir. \n- Kayıt
    işlemlerinin ardından oturum açmalısınız.\n- `API KOD` oluşturabilmek için `VEYOSIS
    PANEL`'e abone olunması ve `IYS KODU`nuzun eklenmiş olması gerekmektedir.\n- `API
    Erişimi` menüsünden Yeni API Kodu oluşturabilirsiniz.\n\n## API Kodu Yetkileri\n\n
    \n\n|        İzin        | Açıklama                                                                                                 |
    \n|:-------------------:| --------------------------------------------------------------------------------------------------------
    | \n | `Marka İşlemleri` | Yetkisi ile marka işlemleri izni verilir.|\n | `İzin
    Yönetimi` | Yetkisi ile izin işlemlerinin tümü gerçekleştirilebilir.|\n | `İzin
    Sorgulama` | Yetkisiyle izin sorgulama işlemlerine izin verilir.|\n | `Ip Kısıtlama`
    | Özelliği aktif edildiğinde istenilen IP dışındaki adreslerin erişimi engellenir.|
    \n\n ## API Bağlantısı\n\n Oluşturduğunuz API kodunu endpointlere ulaşmak için
    gönderdiğiniz sorgulara `Authorization` header'ı olarak `Bearer XYZXYZXYZ` şeklinde
    eklemeniz gerekiyor."
- name: MARKA İŞLEMLERİ
  description: |-
    Hizmet sağlayıcıya bağlı bulunan markaların listelemesi bu bölümde yapılmaktadır.

    Panel Üzerinden pasifleştirdiğiniz markalarınızı görüntüleyemez ve işlem yapamazsınız.
- name: İZİN YÖNETİMİ
  description: ''
- name: İZİN SORGULAMA
  description: İZİN SORGULAMA
- name: HATA KODLARI
  description: ''
schemes:
- https
paths:
  "/me":
    get:
      tags:
      - KİMLİK DOĞRULAMA
      summary: Kullanıcı Bilgileri
      description: Bu metot API Kodunun yetkili kullanıcısına ait bilgileri listeler.
      consumes:
      - application/json
      produces:
      - application/json
      responses:
        '200':
          description: İşlem Başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  user:
                    type: integer
                    description: Kullanıcı ad ve soyad bilgisi.
                  username:
                    type: string
                    description: Kullanıcı adı bilgisi.
                  email:
                    type: string
                    description: Kullanıcı Eposta bilgisidir.
                  company:
                    type: string
                    description: Kullanıcı hesabına ait firma bilgisi.
                  title:
                    type: string
                    description: Kullanılan API Koduna verilen ad.
                  permissions:
                    type: array
                    description: API Kodu Yetkileri listelenir.[API Kodu Yetkileri](#section/API-Kodu-Yetkileri)
                    enum:
                    - brand
                    - consent
                    - report
                  ip:
                    type: boolean
                    description: API Koduna ait IP Kısıtlamasının olup olmadığı bilgisi.(true
                      / false)
              examples:
                example-1:
                  value:
                    data:
                      user: Örnek Kullanıcı
                      username: kullanici
                      email: mail@example.com
                      company: Örnek Firma Ltd.şti.
                      title: Örnek API
                      permissions:
                      - brand
                      - consent
                      - report
                      ip: false
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: 401
                      message: String
        '402':
          description: Payment Required
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: 402
                      message: Hata kodu ayrıntılı bilgisi.
        '404':
          description: Not Found
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata ile ilgili ayrıntılı bilgi.
              examples:
                example-1:
                  value:
                    error:
                      code: 404
                      message: String
      operationId: getMe
  "/brand/get":
    get:
      tags:
      - MARKA İŞLEMLERİ
      summary: Marka Listeleme
      description: |-
        Bu metot yetkili markaları listelemek için kullanılır.
        Bu metodu kullanabilmek için API kullanıcısına `Marka İşlemleri` yetkisi verilmiş olması gerekmektedir.

        Markalarınızın kodu, isimleri ve izinlerinizin cevabı döner.
      consumes:
      - application/json
      produces:
      - application/json
      responses:
        '200':
          description: İşlem Başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Markaya ait özel marka kodu bilgisi.
                  title:
                    type: string
                    description: Marka başlığı.
                  permissions:
                    description: Metottan dönen izin yetkilerini içeren listedir.
                    type: array
                    items:
                      type: object
                      properties:
                        ARAMA:
                          type: string
                          description: Marka arama yetkisi. (write / read)
                        MESAJ:
                          type: string
                          description: Marka mesaj yetkisi. (write / read)
                        EPOSTA:
                          type: string
                          description: Marka eposta yetkisi. (write / read)
                  consents:
                    description: Metottan dönen izin bilgilerini içeren listedir.
                    type: array
                    items:
                      type: object
                      properties:
                        approval:
                          type: integer
                          description: Markaya ait Onaylı veri toplamı.
                        rejection:
                          type: integer
                          description: Markaya ait Tetli veri toplamı.
                        total:
                          type: integer
                          description: Markaya ait toplam veri toplamı.
              examples:
                example-1:
                  value:
                    data:
                      brands:
                      - code: '00000'
                        title: Organik Haberleşme Teknolojileri bilişim san.ltd.şti.
                        permissions:
                          ARAMA: write
                          MESAJ: write
                          EPOSTA: write
                        consents:
                          approval: 6479
                          rejection: 1
                          total: 6480
                      - code: '000001'
                        title: Organik haberleşme teknolojileri bilişim sanayi ltd.şti.
                        permissions:
                          ARAMA: write
                          MESAJ: write
                          EPOSTA: write
                        consents:
                          approval: 0
                          rejection: 0
                          total: 0
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: 401
                      message: String
        '402':
          description: Payment Required
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: 402
                      message: Hata kodu ayrıntılı bilgisi.
        '404':
          description: Not Found
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata ile ilgili ayrıntılı bilgi.
              examples:
                example-1:
                  value:
                    error:
                      code: 404
                      message: String
      operationId: brandGet
  "/consent/single/{brand}":
    post:
      requestBody:
        description: ''
        content:
          application/json:
            schema:
              type: object
              properties:
                recipient:
                  description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                    veya ```e-posta``` bilgisidir. E-posta adresleri ```265```, telefon
                    numaraları ```15``` karakterden daha uzun olamaz. Telefon numaraları
                    E164 uluslararası([+][country code][area code][local phone number])
                    formata uygun olmalıdır.
                  type: string
                type:
                  description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                  enum:
                  - ARAMA
                  - MESAJ
                  - EPOSTA
                  type: string
                source:
                  description: Vatandaşın izin durumu belirlediği kaynaktır. Alıcı
                    tipi ```TACIR``` ise eklenmesi zorunlu değildir.(TACIR tipli izinlerin
                    güncellemesinde source alanı istek içerisinde yer almalıdır.)
                  enum:
                  - HS_FIZIKSEL_ORTAM
                  - HS_ISLAK_IMZA
                  - HS_WEB
                  - HS_CAGRI_MERKEZI
                  - HS_SOSYAL_MEDYA
                  - HS_EPOSTA
                  - HS_MESAJ
                  - HS_MOBIL
                  - HS_EORTAM
                  - HS_ETKINLIK
                  - HS_2015
                  - HS_ATM
                  - HS_KARAR
                  type: string
                status:
                  description: 'Vatandaşın izin durumunu gösterir. '
                  enum:
                  - ONAY
                  - RET
                  type: string
                consentDate:
                  type: string
                  format: date-time
                  description: İznin, vatandaştan alındığı tarihtir. Alıcı tipi ```TACIR```
                    ise eklenmesi zorunlu değildir. İzinlerin tarih formatı ```YYYY-MM-DD
                    HH:mm:ss```, saat dilimi ```Türkiye saati``` kabul edilir.(TACIR
                    tipli izinlerin güncellemesinde consentDate alanı istek içerisinde
                    yer almalıdır.)
                  example: '2018-02-10 09:30:00'
                recipientType:
                  description: 'İzin kaydının tacir veya bireysel amaçla alındığını
                    ifade eder. Aynı iletişim adresi için bir bireysel, bir tacir
                    izin kaydı oluşturulabilir. Örneğin, perakende sektöründeki bir
                    hizmet sağlayıcıyla ticari bağı bulunan bir muhasebeci aynı zamanda
                    bu hizmet sağlayıcının bireysel müşterisi de olabilir. Dolayısıyla
                    aynı iletişim adresi hem tacir hem de bireysel alıcı olarak İYS''ye
                    kaydedilebilir. (Varsayılan değer: `BIREYSEL`)'
                  enum:
                  - BIREYSEL
                  - TACIR
                  type: string
              required:
              - recipient
              - type
              - source
              - status
              - consentDate
              - recipientType
            examples:
              AddOneConsentRequest:
                value:
                  type: EPOSTA
                  recipientType: BIREYSEL
                  recipient: mail@example.com
                  source: HS_FIZIKSEL_ORTAM
                  consentDate: '2020-12-18 07:07:09'
                  status: ONAY
        required: true
      tags:
      - İZİN YÖNETİMİ
      responses:
        '200':
          content:
            application/json:
              schema:
                description: ''
                type: object
                properties:
                  result:
                    description: İzin yükleme isteğinin sonucudur. (true,false)
                    type: boolean
                required:
                - transactionId
                - creationDate
              examples:
                AddSingleConsentResponse:
                  value:
                    data:
                      result: true
          description: İzin ekleme işlemi başarılı.
        '400':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Bad Request.
        '401':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unauthorized
        '403':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Forbidden
        '404':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Not Found
        '422':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unprocessable entity
        '500':
          description: Internal Server Error
      operationId: consentSingle
      summary: Tekil İzin Ekleme
      description: Vatandaştan alınan tek **bir** izni kaydeder.
    parameters:
    - name: brand
      description: Hizmet sağlayıcının markasına özel kod(code) bilgisidir.
      type: integer
      in: path
      required: true
  "/consent/async/{brand}":
    post:
      requestBody:
        description: ''
        content:
          application/json:
            schema:
              type: object
              properties:
                recipient:
                  description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                    veya ```e-posta``` bilgisidir. E-posta adresleri ```265```, telefon
                    numaraları ```15``` karakterden daha uzun olamaz. Telefon numaraları
                    E164 uluslararası([+][country code][area code][local phone number])
                    formata uygun olmalıdır.
                  type: string
                type:
                  description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                  enum:
                  - ARAMA
                  - MESAJ
                  - EPOSTA
                  type: string
                source:
                  description: Vatandaşın izin durumu belirlediği kaynaktır. Alıcı
                    tipi ```TACIR``` ise eklenmesi zorunlu değildir.(TACIR tipli izinlerin
                    güncellemesinde source alanı istek içerisinde yer almalıdır.)
                  enum:
                  - HS_FIZIKSEL_ORTAM
                  - HS_ISLAK_IMZA
                  - HS_WEB
                  - HS_CAGRI_MERKEZI
                  - HS_SOSYAL_MEDYA
                  - HS_EPOSTA
                  - HS_MESAJ
                  - HS_MOBIL
                  - HS_EORTAM
                  - HS_ETKINLIK
                  - HS_2015
                  - HS_ATM
                  - HS_KARAR
                  type: string
                status:
                  description: 'Vatandaşın izin durumunu gösterir. '
                  enum:
                  - ONAY
                  - RET
                  type: string
                consentDate:
                  type: string
                  format: date-time
                  description: İznin, vatandaştan alındığı tarihtir. Alıcı tipi ```TACIR```
                    ise eklenmesi zorunlu değildir. İzinlerin tarih formatı ```YYYY-MM-DD
                    HH:mm:ss```, saat dilimi ```Türkiye saati``` kabul edilir.(TACIR
                    tipli izinlerin güncellemesinde consentDate alanı istek içerisinde
                    yer almalıdır.)
                  example: '2018-02-10 09:30:00'
                recipientType:
                  description: 'İzin kaydının tacir veya bireysel amaçla alındığını
                    ifade eder. Aynı iletişim adresi için bir bireysel, bir tacir
                    izin kaydı oluşturulabilir. Örneğin, perakende sektöründeki bir
                    hizmet sağlayıcıyla ticari bağı bulunan bir muhasebeci aynı zamanda
                    bu hizmet sağlayıcının bireysel müşterisi de olabilir. Dolayısıyla
                    aynı iletişim adresi hem tacir hem de bireysel alıcı olarak İYS''ye
                    kaydedilebilir. (Varsayılan değer: `BIREYSEL`)'
                  enum:
                  - BIREYSEL
                  - TACIR
                  type: string
              required:
              - recipient
              - type
              - source
              - status
              - consentDate
              - recipientType
            examples:
              consentAsync:
                value:
                - type: MESAJ
                  recipientType: BIREYSEL
                  recipient: '905000000001'
                  source: HS_FIZIKSEL_ORTAM
                  consentDate: '2020-12-10 10:00:00'
                  status: RET
                - type: EPOSTA
                  recipientType: BIREYSEL
                  recipient: mail@example.com
                  source: HS_2015
                  consentDate: '2015-05-01 00:00:00'
                  status: ONAY
        required: true
      tags:
      - İZİN YÖNETİMİ
      responses:
        '200':
          content:
            application/json:
              schema:
                description: ''
                type: object
                properties:
                  transaction:
                    description: Çoklu izin ekleme işlemi kodudur.
                    type: integer
                required:
                - transactionId
              examples:
                AddSingleConsentResponse:
                  value:
                    data:
                      transaction: 47
          description: İzin ekleme işlemi başarılı.
        '400':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Bad Request.
        '401':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unauthorized
        '403':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Forbidden
        '404':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Not Found
        '422':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unprocessable entity
        '500':
          description: Internal Server Error
      operationId: consentAsync
      summary: Çoklu İzin Ekleme
      description: |-
        **Tek** markaya asenkron bir şekilde çoklu izin ekler.

        Tek seferde en fazla 1.000 adet izin yüklenebilir.

        Metotdan dönen `transaction` değeri ile [Çoklu İzin Ekleme Durumu](#operation/consentStatus) metodu ile kuyruk durumunu kontrol edilebilir.
    parameters:
    - name: brand
      description: Hizmet sağlayıcının markasına özel kod(code) bilgisidir.
      type: integer
      in: path
      required: true
  "/consent/status/{transaction}":
    get:
      tags:
      - İZİN YÖNETİMİ
      summary: Çoklu İzin Ekleme Durumu
      description: |-
        Çoklu İzin Ekleme metoduyla eklenen izinlerin durumu kontrol edilir.

        Kontrol işlemi [Çoklu İzin Ekleme](#operation/consentAsync) metodundan dönen `transaction` değeri ile yapılır

        Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.
      consumes:
      - application/json
      produces:
      - application/json
      responses:
        '200':
          description: İşlem Başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  type:
                    description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                    enum:
                    - ARAMA
                    - MESAJ
                    - EPOSTA
                    type: string
                  recipientType:
                    description: 'İzin kaydının tacir veya bireysel amaçla alındığını
                      ifade eder. Aynı iletişim adresi için bir bireysel, bir tacir
                      izin kaydı oluşturulabilir. Örneğin, perakende sektöründeki
                      bir hizmet sağlayıcıyla ticari bağı bulunan bir muhasebeci aynı
                      zamanda bu hizmet sağlayıcının bireysel müşterisi de olabilir.
                      Dolayısıyla aynı iletişim adresi hem tacir hem de bireysel alıcı
                      olarak İYS''ye kaydedilebilir. (Varsayılan değer: `BIREYSEL`)'
                    enum:
                    - BIREYSEL
                    - TACIR
                    type: string
                  recipient:
                    description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                      veya ```e-posta``` bilgisidir. E-posta adresleri ```265```,
                      telefon numaraları ```15``` karakterden daha uzun olamaz. Telefon
                      numaraları E164 uluslararası([+][country code][area code][local
                      phone number]) formata uygun olmalıdır.
                    type: string
                  source:
                    description: Vatandaşın izin durumu belirlediği kaynaktır. Alıcı
                      tipi ```TACIR``` ise eklenmesi zorunlu değildir.(TACIR tipli
                      izinlerin güncellemesinde source alanı istek içerisinde yer
                      almalıdır.)
                    enum:
                    - HS_FIZIKSEL_ORTAM
                    - HS_ISLAK_IMZA
                    - HS_WEB
                    - HS_CAGRI_MERKEZI
                    - HS_SOSYAL_MEDYA
                    - HS_EPOSTA
                    - HS_MESAJ
                    - HS_MOBIL
                    - HS_EORTAM
                    - HS_ETKINLIK
                    - HS_2015
                    - HS_ATM
                    - HS_KARAR
                    type: string
                  consentDate:
                    type: string
                    format: date-time
                    description: İznin, vatandaştan alındığı tarihtir. Alıcı tipi
                      ```TACIR``` ise eklenmesi zorunlu değildir. İzinlerin tarih
                      formatı ```YYYY-MM-DD HH:mm:ss```, saat dilimi ```Türkiye saati```
                      kabul edilir.(TACIR tipli izinlerin güncellemesinde consentDate
                      alanı istek içerisinde yer almalıdır.)
                    example: '2018-02-10 09:30:00'
                  status:
                    description: 'Vatandaşın izin durumunu gösterir. '
                    enum:
                    - ONAY
                    - RET
                    type: string
                  result:
                    description: İzinli verinin sisteme kayıt sonucudur.
                    enum:
                    - success
                    - 'enqueue '
                    - failure
                    type: string
                  error:
                    description: Hata listesidir.
                    type: array
                    items:
                      type: object
                      properties:
                        message:
                          description: Hata nedenini belirtir.
                          type: string
              examples:
                example-1:
                  value:
                    data:
                    - type: MESAJ
                      recipientType: BIREYSEL
                      recipient: "+905000000001"
                      source: HS_FIZIKSEL_ORTAM
                      consentDate: '2020-12-10 10:00:00'
                      status: RET
                      result: success
                    - type: EPOSTA
                      recipientType: BIREYSEL
                      recipient: mail@example.com
                      source: HS_2015
                      consentDate: '2015-05-01 00:00:00'
                      status: ONAY
                      result: failure
                      error:
                        message: Sistemdeki iznin tarihinden önceki tarihli izinlerle
                          güncelleme yapılamaz.
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                transactionStatus:
                  value:
                    error:
                      code: 401
                      message: String
        '402':
          description: Payment Required
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: 402
                      message: Hata kodu ayrıntılı bilgisi.
        '404':
          description: Not Found
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata ile ilgili ayrıntılı bilgi.
              examples:
                example-1:
                  value:
                    error:
                      code: 404
                      message: String
      operationId: consentStatus
    parameters:
    - name: transaction
      description: Çoklu izin ekleme metodundan dönen `transaction` kodu.
      type: integer
      in: path
      required: true
  "/report/single/{brand}":
    post:
      requestBody:
        description: ''
        content:
          application/json:
            schema:
              type: object
              properties:
                type:
                  description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                  enum:
                  - ARAMA
                  - MESAJ
                  - EPOSTA
                  type: string
                recipientType:
                  description: 'İzin kaydının tacir veya bireysel amaçla alındığını
                    ifade eder. Aynı iletişim adresi için bir bireysel, bir tacir
                    izin kaydı oluşturulabilir. Örneğin, perakende sektöründeki bir
                    hizmet sağlayıcıyla ticari bağı bulunan bir muhasebeci aynı zamanda
                    bu hizmet sağlayıcının bireysel müşterisi de olabilir. Dolayısıyla
                    aynı iletişim adresi hem tacir hem de bireysel alıcı olarak İYS''ye
                    kaydedilebilir. (Varsayılan değer: `BIREYSEL`)'
                  enum:
                  - BIREYSEL
                  - TACIR
                  type: string
                recipient:
                  description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                    veya ```e-posta``` bilgisidir. E-posta adresleri ```265```, telefon
                    numaraları ```15``` karakterden daha uzun olamaz. Telefon numaraları
                    E164 uluslararası([+][country code][area code][local phone number])
                    formata uygun olmalıdır.
                  type: string
              required:
              - type
              - recipientType
              - recipient
            examples:
              reportSingle:
                value:
                  type: EPOSTA
                  recipientType: BIREYSEL
                  recipient: mail@example.com
        required: true
      tags:
      - İZİN SORGULAMA
      responses:
        '200':
          content:
            application/json:
              schema:
                description: ''
                type: object
                properties:
                  recipient:
                    description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                      veya ```e-posta``` bilgisidir.
                    type: string
                  type:
                    description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                    enum:
                    - ARAMA
                    - MESAJ
                    - EPOSTA
                    type: string
                  source:
                    description: Vatandaşın izin durumu belirlediği kaynaktır.
                    enum:
                    - HS_FIZIKSEL_ORTAM
                    - HS_ISLAK_IMZA
                    - HS_WEB
                    - HS_CAGRI_MERKEZI
                    - HS_SOSYAL_MEDYA
                    - HS_EPOSTA
                    - HS_MESAJ
                    - HS_MOBIL
                    - HS_EORTAM
                    - HS_ETKINLIK
                    - HS_2015
                    - HS_ATM
                    - HS_KARAR
                    type: string
                  status:
                    description: 'Vatandaşın izin durumunu gösterir. '
                    enum:
                    - ONAY
                    - RET
                    type: string
                  consentDate:
                    type: string
                    format: date-time
                    description: İznin, vatandaştan alındığı tarihtir.
                    example: '2018-02-10 09:30:00'
                  recipientType:
                    description: İzin kaydının tacir veya bireysel amaçla alındığını
                      ifade eder.
                    enum:
                    - BIREYSEL
                    - TACIR
                    type: string
                required:
                - transactionId
                - creationDate
              examples:
                AddSingleConsentResponse:
                  value:
                    recipient: mail@example.com
                    type: EPOSTA
                    source: HS_FIZIKSEL_ORTAM
                    status: ONAY
                    consentDate: '2020-12-18 07:07:09'
                    recipientType: BIREYSEL
          description: İşlemi başarılı.
        '400':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Bad Request.
        '401':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unauthorized
        '403':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Forbidden
        '404':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Not Found
        '422':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Unprocessable entity
        '500':
          description: Internal Server Error
      operationId: reportSingle
      summary: Tekil İzin Sorgulama
      description: Tek bir iznin durumunu kontrol eden metoddur.
    parameters:
    - name: brand
      description: Hizmet sağlayıcının markasına özel kod(code) bilgisidir.
      type: integer
      in: path
      required: true
  "/report/async/{brand}":
    post:
      requestBody:
        description: ''
        content:
          application/json:
            schema:
              type: object
              properties:
                type:
                  description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                  enum:
                  - ARAMA
                  - MESAJ
                  - EPOSTA
                  type: string
                recipientType:
                  description: 'İzin kaydının tacir veya bireysel amaçla alındığını
                    ifade eder. Aynı iletişim adresi için bir bireysel, bir tacir
                    izin kaydı oluşturulabilir. Örneğin, perakende sektöründeki bir
                    hizmet sağlayıcıyla ticari bağı bulunan bir muhasebeci aynı zamanda
                    bu hizmet sağlayıcının bireysel müşterisi de olabilir. Dolayısıyla
                    aynı iletişim adresi hem tacir hem de bireysel alıcı olarak İYS''ye
                    kaydedilebilir. (Varsayılan değer: `BIREYSEL`)'
                  enum:
                  - BIREYSEL
                  - TACIR
                  type: string
                recipient:
                  description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                    veya ```e-posta``` bilgisidir. E-posta adresleri ```265```, telefon
                    numaraları ```15``` karakterden daha uzun olamaz. Telefon numaraları
                    E164 uluslararası([+][country code][area code][local phone number])
                    formata uygun olmalıdır.
                  type: string
              required:
              - type
              - recipientType
              - recipient
            examples:
              reportSingle:
                value:
                  type: MESAJ
                  recipientType: BIREYSEL
                  recipients:
                  - '905000000001'
                  - '905000000002'
        required: true
      tags:
      - İZİN SORGULAMA
      responses:
        '200':
          content:
            application/json:
              schema:
                description: ''
                type: object
                properties:
                  transaction:
                    description: Çoklu izin sorgulama durumu işlemi kodudur.
                    type: integer
                required:
                - transactionId
              examples:
                AddSingleConsentResponse:
                  value:
                    data:
                      transaction: 47
          description: İşlemi başarılı.
        '400':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: 0
                    message: String
          description: Bad Request.
        '401':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: '0'
                    message: String
          description: Unauthorized
        '403':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: '0'
                    message: String
          description: Forbidden
        '404':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: '0'
                    message: String
          description: Not Found
        '422':
          content:
            application/json:
              schema:
                "$ref": "#/definitions/errorResponses"
              examples:
                missingToken:
                  value:
                    code: '0'
                    message: String
          description: Unprocessable entity
        '500':
          description: Internal Server Error
      operationId: reportAsync
      summary: Çoklu İzin Sorgulama
      description: |-
        Çoklu izin durumunu kontrol eden metoddur.

        Tek seferde en fazla 1.000 adet izin yüklenebilir.
    parameters:
    - name: brand
      description: Hizmet sağlayıcının markasına özel kod(code) bilgisidir.
      type: integer
      in: path
      required: 'true'
  "/report/status/{transaction}":
    get:
      tags:
      - İZİN SORGULAMA
      summary: Çoklu İzin Sorgulama Durumu
      description: |-
        Çoklu İzin Sorgulama metoduyla sorgulanan izinlerin durumu kontrol edilir.

        Kontrol işlemi [Çoklu İzin Sorgulama](#operation/reportAsync) metodundan dönen `transaction` değeri ile yapılır

        Çoklu veri ekleme sonuçları sistemde **1**(bir) hafta tutulur.
      consumes:
      - application/json
      produces:
      - application/json
      responses:
        '200':
          description: İşlem Başarılı
          content:
            application/json:
              schema:
                type: object
                properties:
                  recipient:
                    description: Vatandaşın sistemde kayıtlı ```telefon numarası```
                      veya ```e-posta``` bilgisidir.
                    type: string
                  type:
                    description: 'Vatandaşın izin verdiği iletişim kanalıdır. '
                    enum:
                    - ARAMA
                    - MESAJ
                    - EPOSTA
                    type: string
                  source:
                    description: Vatandaşın izin durumu belirlediği kaynaktır.
                    enum:
                    - HS_FIZIKSEL_ORTAM
                    - HS_ISLAK_IMZA
                    - HS_WEB
                    - HS_CAGRI_MERKEZI
                    - HS_SOSYAL_MEDYA
                    - HS_EPOSTA
                    - HS_MESAJ
                    - HS_MOBIL
                    - HS_EORTAM
                    - HS_ETKINLIK
                    - HS_2015
                    - HS_ATM
                    - HS_KARAR
                    type: string
                  status:
                    description: 'Vatandaşın izin durumunu gösterir. '
                    enum:
                    - ONAY
                    - RET
                    type: string
                  consentDate:
                    type: string
                    format: date-time
                    description: İznin, vatandaştan alındığı tarihtir.
                    example: '2018-02-10 09:30:00'
                  recipientType:
                    description: İzin kaydının tacir veya bireysel amaçla alındığını
                      ifade eder.
                    enum:
                    - BIREYSEL
                    - TACIR
                    type: string
                  error:
                    description: Metottan dönen hata listesidir.
                    type: array
                    items:
                      type: object
                      properties:
                        message:
                          description: Metottan dönen hatanın açıklama bilgisidir.
                          type: string
              examples:
                example-1:
                  value:
                    data:
                    - type: MESAJ
                      recipientType: BIREYSEL
                      recipient: '905000000001'
                      source: HS_FIZIKSEL_ORTAM
                      consentDate: '2020-12-14 16:46:00'
                      status: ONAY
                    - recipient: '905555555555'
                    - recipient: '90555'
                      error:
                        message: Alıcı (recipient) için E164 uluslararası([+][country
                          code][area code][local phone number]) formatına uygun bir
                          telefon numarası girilmelidir. (örn. `+905992000000`)
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                transactionStatus:
                  value:
                    error:
                      code: '401'
                      message: String
        '402':
          description: Payment Required
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata kodu ayrıntılı bilgisi.
              examples:
                example-1:
                  value:
                    error:
                      code: '402'
                      message: Hata kodu ayrıntılı bilgisi.
        '404':
          description: Not Found
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                    description: Hata kodu bilgisi.
                  message:
                    type: string
                    description: Hata ile ilgili ayrıntılı bilgi.
              examples:
                example-1:
                  value:
                    error:
                      code: '404'
                      message: String
      operationId: reportStatus
    parameters:
    - name: transaction
      description: Çoklu izin sorgulama metodundan dönen `transaction` kodu.
      type: integer
      in: path
      required: 'true'

