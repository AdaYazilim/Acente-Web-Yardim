# Ada Yazılım Acente Cari WEB API

Sistemimiz, linklere gerekli parametrelerin json yapısında gönderilmesi üzerinde kuruludur. Login gerekli olan işlemler için öncelikle login isteğine başarılı cevap almalısınız, ardından yetki gerektiren işlemi yapabilirsiniz. Dikkat edilmesi gereken konu ise bu işlemlerin aynı sessionda yapılmasıdır.

###1. Kullanıcı Girişi

**Link:**"http://localhost/ada/Login.GirisYap.aaw"

**Parametreler:** KullanıcıAdı ve Kullanıcı şifresi (Bu bilgileri acenteden temin edebilirsiniz.)

##### Örnek İstek:

["kullaniciAdi","Sifre"]

##### **Örnek Cevap:**

{"Basarili":true,"Mesaj":""}

###2. Poliçe Arama

**Link:**"http://localhost/ada/Police.PoliceAra.aaw"

**Parametreler:** Query (Bu bilgi " **PoliceNo/SigortalıAdı/KimlikNo/Plaka/Marka**"  alanları için geçerlidir. yazacağınız query bilgisi bu alanlarla karşılaştırılır ve uygun kayıtlar getirilir.)

##### **Örnek İstek:**

    ["0330011122"]

##### Örnek Cevap:

    [{
     "FprkPol": 176529, //Cari veritabanında kayıtlı poliçe keyidir.
     "Tanzim": "2015-03-24T00:00:00", //Poliçe Tanzim Tarihidir.
     "Baslangic": "2015-03-20T00:00:00", //Poliçe Başlangıç Tarihidir.
     "Bitis": "2015-03-20T00:00:00", //Poliçe Bitiş Tarihidir.
     "Brans":"412", // Cari veritabanında Tanımlı Poliçe Brans Kodudur.
     "PolGrp":"NAK", // Poliçe Grubudur.
     "PoliceNo": "24370032", //Şirket Poliçe Numarasıdır.
     "TecditNo":"", //Şirket Tecdit Numarasıdır.
     "ZeyilNo": "1", //Şirket Zeyil Numarasıdır.
     "FfrkSir": 26, //Cari veritabanında kayıtlı Şirket Kodudur.
     "FfrkMus": 73376, //Cari veritabanında kayıtlı Müşteri keyidir.
     "Sigortali": "ABC TEKSTİL İHRACAT PAZARLAMA A.Ş.",Poliçede Kayıtlı Sigortalı Adıdır.
     "Plaka": "",//Poliçede Kayıtlı Plaka bilgisidir. Bazı Poliçe gruplarında boş gelir.
     "TeklifDurumu": 0,
     "Il": 34,//Poliçenin İl Bilgisi
     "Ilce": "GÜMÜŞSUYU",//Poliçenin İlçe Bilgisi
     "FyrlkDrm": 2, //Poliçe Yürürlük Durumu
     "PolTek": 1,//Poliçe Sınıfı
     "FfrkTt": 0,//Tecdit Tipi
     "FpolTip": 0, //Poliçe Tipi
     "FTcKimNo": "",//Sigortalı TC Kimlik Numarası
     "FVerKimNo": "0330011122", //Sigortalı VergiNumarası
     "Marka": "",//Poliçede Kayıtlı Araç Marka bilgisidir. Bazı Poliçe gruplarında boş gelir.
     "FfrkMizBlg": 0//ŞubeKodu
    }]

###3. Detaylı Poliçe Arama
**Link:**"http://localhost/ada/Police.DetayliPoliceArama.aaw"

**Parametreler:**

_Sorgu:_ Poliçe numarası, KimlikNo, VergiNo, Sigortalı, Plaka, Marka bilgileri için ortak alandır.

_EkAlanlar:_ Standart alanlara ek olarak istenen diğer alanlar liste olarak alınır. Poliçe alanları başlığından eklemeler yapabilirsiniz. Eksik alanlar için iletişime geçebilirsiniz.

_Yururlukte:_ Poliçenin yürürlükte durumunu belirtir. Alttaki değerleri alabilir. Acente Cari porogramından toplu olarak güncellenmekte olduğu unutulmamalıdır.

|  Yürürlük Durumu | Kod |
| --- | --- |
| Hepsi (varsayılan) | 0 |
| Yürürlükte | 1 |
| Yürürlükte Değil | 2 |

Not: Poliçe aramada gelen sonuçlardaki alan adlarına göre farklılıklar vardır.

##### Örnek İstek:

    {Sorgu:'78545625859', EkAlanlar:['feposta','ftelno','SirketAdi','fsigdogtar','fsigetdtar'],Yururlukte:1}

##### Örnek Cevap:

    [{
        "Alanlar": [
            {
                "Soru": "fPrkPol",
                "Cevap": "38"
            },
            {
                "Soru": "fTnzTar",
                "Cevap": "29.09.2006"
            },
            {
                "Soru": "Fbastar",
                "Cevap": ""
            },
            ...
        ],
            "OdemePlani": {
        "Taksitler": [
            {
                "FPrkOpl": 1259,
                "fFrkPol": 0,
                "MusteriTarihi": "2006-11-06T00:00:00",
                "MusteriOdemeTutari": {},
                "MusteriIptalTutari": {},
                "AcenteTarihi": "2007-01-04T00:00:00",
                "AcenteTutar": {},
                "Kapamalar": [
                    {
                        "TahsilatList": [
                            {
                                "SimdiyeKadarKapananTutar": {},
                                "TahsilatTutari": {},
                                "KapamaIcinKullanilabilecekTutar": {},
                                "Alanlar": {
                                    "fAci": {
                                        "$type": "AdaGenel.Cesitli.NesneAlan, AdaGenel",
                                        "SaltOkunur": false,
                                        "Soru": "fAci",
                                        "Cevap": "(00118561-1) POLİÇE KAPAMASI                                    "
                                    },
                                    ...
                                }
                            }
                        ],
                        "KapananTutar": 206.2
                    }
                ],
                "PoliceNo": null,
                "TecditNo": null,
                "ZeyilNo": null,
                "Sirket": null,
                "TaksitKodu": null,
                "Sigortali": null,
                "Plaka": null,
                "MusteriOdemesiGerekenTutar": {},
                "ToplamKapananTutar": {},
                "IlgiliTahsilatIleKapananTutar": {},
                "KalanBorcu": {},
                "Isaretli": false,
                "IlkGiristeIsaretli": false,
                "IlkGiristeKapaliTutar": {}
            }},
            ...
        ]
    },
    "Hasarlar": [],
    "Teminatlar": [],
    "Sigortalilar": [
        {
            "Ad": "BURCU",
            "Soyad": "İNANÇ",
            "Cinsiyet": "K",
            "KimlikNo": "22870344190",
            "Yakinlik": 1,
            "DogumTarihi": "1957-07-30T00:00:00"
        },
        ...    
        ]
    }
    ]

###4. Poliçe Bilgilerini Al

**Link:**"http://localhost/ada/Police.PoliceBilgileriniAl.aaw"

**Parametreler:** adayazılım sisteminde tanımlı poliçe numarası (fprkPol) ve istenen ek alanlar gönderilmelidir.

##### Örnek İstek:


    [3488,['feposta','ftelno','SirketAdi','fsigdogtar','fsigetdtar']]

##### Örnek Cevap:


    {
    "Alanlar": [
        {
            "Soru": "fPrkPol",
            "Cevap": "3488"
        },
        {
            "Soru": "fTnzTar",
            "Cevap": "04.01.2007"
        },
        {
            "Soru": "Fbastar",
            "Cevap": ""
        },
        {
            "Soru": "Fbittar",
            "Cevap": ""
        },
        {
            "Soru": "Brans",
            "Cevap": "608"
        },
        {
            "Soru": "PolGrp",
            "Cevap": "SAG"
        },
        ...
    ],
    "OdemePlani": {
        "Taksitler": [
            {
                "FPrkOpl": 1259,
                "fFrkPol": 0,
                "MusteriTarihi": "2006-11-06T00:00:00",
                "MusteriOdemeTutari": {},
                "MusteriIptalTutari": {},
                "AcenteTarihi": "2007-01-04T00:00:00",
                "AcenteTutar": {},
                "Kapamalar": [
                    {
                        "TahsilatList": [
                            {
                                "SimdiyeKadarKapananTutar": {},
                                "TahsilatTutari": {},
                                "KapamaIcinKullanilabilecekTutar": {},
                                "Alanlar": {
                                    "fAci": {
                                        "$type": "AdaGenel.Cesitli.NesneAlan, AdaGenel",
                                        "SaltOkunur": false,
                                        "Soru": "fAci",
                                        "Cevap": "(00935421-1) POLİÇE KAPAMASI                                    "
                                    },
                                    ...
                                }
                            }
                        ],
                        "KapananTutar": 206.2
                    }
                ],
                "PoliceNo": null,
                "TecditNo": null,
                "ZeyilNo": null,
                "Sirket": null,
                "TaksitKodu": null,
                "Sigortali": null,
                "Plaka": null,
                "MusteriOdemesiGerekenTutar": {},
                "ToplamKapananTutar": {},
                "IlgiliTahsilatIleKapananTutar": {},
                "KalanBorcu": {},
                "Isaretli": false,
                "IlkGiristeIsaretli": false,
                "IlkGiristeKapaliTutar": {}
            }},
            ...
        ]
    },
    "Hasarlar": [],
    "Teminatlar": [],
    "Sigortalilar": [
        {
            "Ad": "BURCU",
            "Soyad": "İNANÇLı",
            "Cinsiyet": "K",
            "KimlikNo": "22720347190",
            "Yakinlik": 1,
            "DogumTarihi": "1957-07-30T00:00:00"
        },
        {
            "Ad": "HİLMİ",
            "Soyad": "İNANÇLI",
            "Cinsiyet": "E",
            "KimlikNo": "24443304756",
            "Yakinlik": 3,
            "DogumTarihi": "1982-05-31T00:00:00"
        },
        {
            "Ad": "BUSE",
            "Soyad": "İNANÇLI",
            "Cinsiyet": "K",
            "KimlikNo": "24731845762",
            "Yakinlik": 3,
            "DogumTarihi": "1985-07-03T00:00:00"
        }
    ]
    }

###5. Poliçe Ekleme

**Link:**" [http://localhost/ada/Police.PoliceEkle.aaw"

**Parametreler:** poliçenin bilgilerini SoruCevap listesi olarak almaktadır. Sorular ve açıklamalarına "Poliçe Alanları ve Karşılıkları" alanından ulaşabilirsiniz. Örnekte gösterilen alanlar zorunlu alanlardır.

##### Örnek İstek:

    [
    [
        {
            "Soru": "SigAdi",
            "Cevap": "Deneme Müşterisi"
        },
        {
            "Soru": "fSirPolNo",
            "Cevap": "4587456"
        },
        {
            "Soru": "fFrkMus",
            "Cevap": "1"
        },
        {
            "Soru": "fFrkSir",
            "Cevap": "1"
        },
        {
            "Soru": "fTnzTar",
            "Cevap": "04.01.2007"
        },
        {
            "Soru": "fTnzTar",
            "Cevap": "04.01.2007"
        },
        {
            "Soru": "fBasTar",
            "Cevap": "04.01.2007"
        },
        {
            "Soru": "fBitTar",
            "Cevap": "04.01.2008"
        },
        {
            "Soru": "Brans",
            "Cevap": "608"
        },
        {
            "Soru": "PolGrp",
            "Cevap": "SAG"
        },
        {
            "Soru": "fPolTek",
            "Cevap": "3"
        },
        ....
    ]
    ]

##### Örnek Cevap:

    {
        "Prk": 239759,
        "Basarili": true,
        "Mesaj": null
    }

###6. Müşteri Arama

**Link:**"http://localhost/ada/Musteri.MusteriAra.aaw"

**Parametreler:** Sorgu, Müşteri Tipi ve istenen ek alanlar gönderilmelidir. Sorgu alanı ad ve soyad, TC Kimlik ve Vergi Numarasına göre arama yapmaktadır. Müşteri Tipi alanı null girilirse tipten bağımsız arama yapmaktadır. Alabildiği değerler "Bazı alanlar ve Alabildiği Değerler" kısmında belirtilmiştir.

##### Örnek İstek:

    [{Sorgu:'',MusteriTipi:null,EkAlanlar:["fMusKimNo","fMusTcKimN"]}]

##### Örnek Cevap:

    [
    {
        "Alanlar": [
            {
                "Soru": "fKulHesKod",
                "Cevap": ""
            },
            {
                "Soru": "fMusAdi",
                "Cevap": ""
            },
            {
                "Soru": "fMusKimNo",
                "Cevap": ""
            },
            {
                "Soru": "fMusSoyadi",
                "Cevap": ""
            },
            {
                "Soru": "fMusTcKimN",
                "Cevap": ""
            },
            {
                "Soru": "fMusTip",
                "Cevap": "1"
            },
            {
                "Soru": "fPrkMus",
                "Cevap": "18580"
            }
        ]
    },
    {
        "Alanlar": [
            {
                "Soru": "fKulHesKod",
                "Cevap": "1.1.1463"
            },
            {
                "Soru": "fMusAdi",
                "Cevap": "deneme Müşterisi"
            },
            {
                "Soru": "fMusKimNo",
                "Cevap": ""
            },
            {
                "Soru": "fMusSoyadi",
                "Cevap": ""
            },
            {
                "Soru": "fMusTcKimN",
                "Cevap": "00000078842"
            },
            {
                "Soru": "fMusTip",
                "Cevap": "1"
            },
            {
                "Soru": "fPrkMus",
                "Cevap": "12841"
            }
    ]

###7. Müşteri Bilgilerini Al

**Link:**"http://localhost/ada/Musteri.MusteriAl.aaw"

**Parametreler:** Müşteri Numarası ve ek olarak istenen Müşteri ek alanları Gönderilir..

##### Örnek İstek:

    [19828, ["fMusKimNo","fMusTcKimN"]]

##### Örnek Cevap:

    {
    "Nesne": {
        "Alanlar": [
            {
                "Soru": "fPrkMus",
                "Cevap": "19828"
            },
            {
                "Soru": "fmuskod",
                "Cevap": ""
            },
            {
                "Soru": "fMusAdi",
                "Cevap": "zerrin.."
            },
            {
                "Soru": "fMusSoyadi",
                "Cevap": ""
            },
            {
                "Soru": "fMusUnv",
                "Cevap": ""
            },
            {
                "Soru": "fMusKimNo",
                "Cevap": ""
            },
            {
                "Soru": "fMusTcKimN",
                "Cevap": ""
            },
            {
                "Soru": "fMusTip",
                "Cevap": "1"
            },
            {
                "Soru": "fMusKimNo",
                "Cevap": ""
            },
            {
                "Soru": "fMusTcKimN",
                "Cevap": ""
            }
        ]
    },
    "Basarili": true,
    "Mesaj": ""
    }

###8. Müşteri Ekle

**Link:**"http://localhost/ada/Musteri.MusteriEkle.aaw"

**Parametreler:** Eklemek istediğiniz Müşteriye ait alanları Soru Cevap listesi olarak göndermeniz gerekmektedir

Not: İnternet Müşterisi olarak işaretlenmek istenen müşterilerde "fIntntMus" alanının değeri 1 olarak ayarlanmalıdır.

##### Örnek İstek:

    [[
       {
        Soru: "fMusAdi",
         Cevap: "zerrin.."
       },
       {
        Soru: "fIntntMus",
        Cevap: "1"
       },
        ....
 ]]

##### Örnek Cevap:

    {
    "Prk": 19829,
    "Basarili": true,
    "Mesaj": null
    }

###9. Müşteri Güncelle

**Link:**"http://localhost/ada/Musteri.MusteriGuncelle.aaw"

**Parametreler:** Güncellemek istediğiniz Müşteriye ait alanları Soru Cevap listesi olarak göndermeniz gerekmektedir. Gönderdiğiniz alanlar güncellenecektir diğer alanlarda herhangibir değişiklik yapılmayacaktır. Soru cevap Listenizde "fPrkMus" alanı ile müşteri numarasının belirtilmesi zorunludur.

##### Örnek İstek:

    [[
    {Soru:"fPrkMus",Cevap:"19829"},
    {Soru:"fMusAdi",Cevap:"zerrin.."},
    ...
    ]]

##### Örnek Cevap:

    {
        "Prk": 19829,
        "Basarili": true,
        "Mesaj": null
    }

###10. Ek Servisler

Sistemdeki bazı sabitlerin değerlerinin alınabilmesi için hazırlanmış bazı fonksiyonlar ve tablolar vardır.

#### Sigorta Şirketleri

_Link:_ "http://localhost/ada/CariSabitleri.SigortaSirketleri.aaw"

_Parametreler:_ parametre almamaktadır.

##### Örnek İstek:

    []

##### Örnek Cevap:

    [
         {
            "PrkSir": 54,// adayazılımın verdiği şirket kodu
            "BirlikKod": 5,//Birlik şirket Kodu
            "SirKod": "AIG",//Şirket Kodu
            "SirAdi": "AIG SİGORTA" // Şirket Adı
        }                              
    ]

#### Araç Kullanım Tarzları

_Link:_ "http://localhost/ada/CariSabitleri.AracKullanimTarzlari.aaw"

_Parametreler:_ parametre almamaktadır.

#####Örnek İstek:

    []

#### Örnek Cevap:

    [
        {
            "TarzKodu": 16,// adayazılımın verdiği Tarzkodu
            "TarzAdi": "Çekici"// tarz adı
        },
         ...
    ]

#### Branşlar

Acente sisteminde tanımlı branş bilgilerinin alınmasını sağlayan servistir.

_Link:_ "http://localhost/ada/CariSabitleri.Branslar.aaw"

_Parametreler:_ parametre almamaktadır.

##### Örnek İstek:

    []

##### Örnek Cevap:

    [
       {
            "fPrkBrn": 1100100,
            "fBrnKod": "100",// adayazılımın verdiği Branş Kodu
            "fAltBrnKod": "100",// adayazılımın verdiği Alt Branş Kodu
            "fSirBrnkod": "1350",//Şirketin verdiği BranşKodu
            "fBrnAdi": "YANGIN KOMBİNE",// adayazılımın verdiği Branş Adı
            "fDomPolGrp": "YNG",// adayazılımın verdiği Branş Grubu
            "fFrkSir": "1",// adayazılımın verdiği Şirket kodu
            "fAcoBrnAd2": "YANGIN",//Acentenin Verdiği Branş Adı 2
            "fAcoBrnKod": "YNG",//Acentenin Verdiği Branş Kodu
            "fAcoBrnAdi": "YANGIN"//Acentenin Verdiği Branş Adı
        }
    ]

### Şirket Branşlarının Tanımlarını Al

Şirket branşlarındaki Teminat tanımlarınınalınmasını sağlayan servistir.

_Link:_ "http://localhost/ada/CariSabitleri.SirketBranslarininTanimlariniAl.aaw"

_Parametreler:_ adayazılımın verdiği Şirket kodu ve adayazılımın verdiği Branş Kodu

#### Örnek İstek:

    ['1','100']

#### Örnek Cevap:

    [
        {
            "fPrkTei": 1,
            "fFrkBrn": "1100100",
            "TAdi": "YANGIN BİNA",
            "tKodu": "011",
            "fBina": true,
            "fEsya": false,
            "fAracTem": false
        },
        ...
    ]

###11. Sistem Ek Özellikleri

##### Aynı Oturumda Birden Fazla İstekde Bulunma

Aynı oturumda ".aaw" uzantısı ile çağıracağınız iki istek sırayla çalışmaktadır. Aynı anda yapacağınız istekleriniz için ".aaws" uzantısı ile istekte bulunabilirsiniz(".aaws" uzantısı session yazım izni olmadığından, bazı servislerde hata verebilir).

##### Birden Fazla Cari Veritabanı Kulanma

Bazı acentelerimiz Cari programda birden fazla veritabanı kullanmaktadır. Farklı veritabanlarında işlem yapmak için, isteklerinize Headers olarak "VeritabaniNumarasi" değerini eklemeniz gerekmektedir. Alabileceği değerler acenteye göre değişikilk gösterdiğinden, bu bilgileri, acenteniz aracılığı ile adayazılım dan öğrenebilirsiniz. Gerekirse tanımlama işlemleride yapılarak size bilgi verilecektir.

###12. Alanlar ve Açıklamaları

##### Poliçe Alanları ve Karşılıkları

<table>
<tr><th colspan='2'>Genel Alanlar</th></tr>
<tr><td>fPrkPol</td><td>Adayazılımın verdiği poliçe numarası</td></tr>
<tr><td>fTnzTar</td><td>Tanzim tarihi</td></tr>
<tr><td>fBasTar</td><td>Başlangıç tarihi</td></tr>
<tr><td>fBitTar</td><td>Bitiş tarihi</td></tr>
<tr><td>Brans</td><td>Adayazılımın verdiği Branş</td></tr>
<tr><td>PolGrp</td><td>Adayazılımın verdiği Poliçe grubu</td></tr>
<tr><td>fSirPolNo</td><td>Şirketin Verdiği Poliçe Numarası</td></tr>
<tr><td>Tecdit</td><td>| Tecdit Numarası</td></tr>
<tr><td>Zeyl</td><td>| Zeyil Numarası</td></tr>
<tr><td>fFrkSir</td><td>Adayazılımın verdiği şirket Kodu</td></tr>
<tr><td>fFrkMus</td><td>Müşteri Numrası</td></tr>
<tr><td>fFrkTa</td><td>Tali Müşteri Numarası</td></tr>
<tr><td>fFrkSat</td><td>Satıcı Müşteri Numarası</td></tr>
<tr><td>fFrkSor</td><td>Sorumlu Müşteri Numarası</td></tr>
<tr><td>SigAdi</td><td>Şirket Adı</td></tr>
<tr><td>Plaka</td><td>Plaka</td></tr>
<tr><td>fTeklifDrm</td><td>Teklif Durumu</td></tr>
<tr><td>fYrlkDrm</td><td>Yürürlük Durumu</td></tr>
<tr><td>fPolTek</td><td>Poliçe Sınıfı</td></tr>
<tr><td>fPolTip</td><td>Poliçe Tipi</td></tr>
<tr><td>fVerKimNo</td><td>Vergi Numarası</td></tr>
<tr><td>fTCKimNo</td><td>TC Kimlik Numarası</td></tr>
<tr><td>fFrkMizBlg</td><td>Müşterinin Bölgesi</td></tr>
<tr><td>fBrtPrm</td><td>Bürüt Prim</td></tr>
<tr><td>RizIl</td><td>Riziko İl</td></tr>
<tr><td>RizIlce</td><td>Riziko İlçe</td></tr>
<tr><td>fPolAltGrp</td><td>Alt Brans Grubu (örn; KNT)</td></tr>
</table>


<table>
<tr><th colspan='2'>Ek Alanlar</th></tr>
<tr><td>feposta</td><td>Eposta Adresi</td></tr>
<tr><td>ftelno</td><td>Telefon Numarası</td></tr>
<tr><td>SirketAdi</td><td>Şirket Adı</td></tr>
<tr><td>fsigdogtar</td><td>Sigortalı Doğum Tarihi</td></tr>
<tr><td>fsigetdtar</td><td>Sigorta Ettiren Doğum Tarihi</td></tr>
</table>

<table>
<tr><th colspan='2'>Trafik Kasko Ek Alanları</th></tr>
<tr><td>fegmblgno</td><td>EgmBelge Numarası/Asbis numarası</td></tr>
<tr><td>fArcKod</td><td>Araç Kodu</td></tr>
<tr><td>Marka</td><td>Araç Marka</td></tr>
<tr><td>Model</td><td>Araç Model</td></tr>
<tr><td>ModYil</td><td>Araç Model Yılı</td></tr>
</table>

<table>
<tr><th colspan='2'>Dask Ek Alanları</th></tr>
<tr><td>fTopKatSay</td><td>Toplam Kat Sayısı</td></tr>
<tr><td>fBrtM2</td><td>Bürüt Metre Kare</td></tr>
<tr><td>fInsYil</td><td>İnşaat Yılı</td></tr>
<tr><td>fuavtadres</td><td>UAVT adres Kodu</td></tr>
<tr><td>fRizDaire</td><td>Riziko Adresi</td></tr>
<tr><td>fDskPolNo</td><td>Dask Poliçe Numarası</td></tr>
</table>

<table>
<tr><th colspan='2'>Bes Ek Alanları</th></tr>
<tr><td>fTaksitTut</td><td>Taksit Tutarı</td></tr>
<tr><td>fFonTutar</td><td>Fon Tutarı</td></tr>
<tr><td>fEkKatPay</td><td>Aylık Katkı Payı</td></tr>
<tr><td>fBasKatPay</td><td>Başlangıç Katkı Payı</td></tr>
<tr><td>fDevAidat</td><td>Devreden Katkı Payı</td></tr>
<tr><td>fYilPrm</td><td>Yıllık Prim</td></tr>
<tr><td>fBesOdeSek</td><td>Ödeme Şekli</td></tr>
<tr><td>fTakBnkSic</td><td>Takas Bank Sicil Numarası</td></tr>
<tr><td>fSozFonTut</td><td>Fon Dağılımı</td></tr>
</table>

##### Müşteri Alanları ve Karşılıkları

<table>
<tr><th colspan='2'>Genel Alanlar</th></tr>
<tr><td>fPrkMus</td><td>Müşteri Numarası</td></tr>
<tr><td>fMusAdi</td><td>Adı</td></tr>
<tr><td>fMusSoyadi</td><td>Soyadı</td></tr>
<tr><td>fMusTip</td><td>Müşeri Tip</td></tr>
<tr><td>fKulHesKod</td><td> </td></tr>
</table>

<table>
<tr><th colspan='2'>Ek Alanlar</th></tr>
<tr><td>fmuskod</td><td>Müşteri Kodu</td></tr>
<tr><td>fMusUnv</td><td>Ünvanı</td></tr>
<tr><td>fMusKimNo</td><td>VergiNumarası</td></tr>
<tr><td>fMusTcKimN</td><td>TC Kimlik Numarası</td></tr>
<tr><td>fmuscins</td><td>Müşteri Cinsi</td></tr>
<tr><td>fKisiFirma</td><td>Tüzel/Gerçek</td></tr>
<tr><td>fMusHesKod</td><td>Muhasebe Hgesa Kodu</td></tr>
<tr><td>fMusBabaAd</td><td>Baba Adı</td></tr>
<tr><td>fMusUyruk</td><td>Uyruk</td></tr>
<tr><td>fMusAdr1</td><td>Adres 1</td></tr>
<tr><td>fMusAdr2</td><td>Adres 2</td></tr>
<tr><td>fmah</td><td>Mahalle</td></tr>
<tr><td>faptadi</td><td>Apartman Ad</td></tr>
<tr><td>fsemt</td><td>Semt Ad</td></tr>
<tr><td>filKodu</td><td>İl Kodu</td></tr>
<tr><td>fMusIlce</td><td>İlçe Adı</td></tr>
<tr><td>fmusemail</td><td>E-Posta</td></tr>
<tr><td>fMusEvTel</td><td>Ev Telefonu</td></tr>
<tr><td>fMusFax</td><td>Fax</td></tr>
<tr><td>fMusTel</td><td>Telefon</td></tr>
<tr><td>fTel1Aci</td><td>Telefon 1 Açıklaması</td></tr>
<tr><td>fMusTel2</td><td>Telefon 2</td></tr>
<tr><td>fTel2Aci</td><td>Telefon 2 Açıklaması</td></tr>
<tr><td>fmusemail</td><td>Eposta Adresi</td></tr>
<tr><td>fIntntMus</td><td>İnternet Müşterisi mi(1/0)</td></tr>
</table>

##### Bazı alanlar ve Alabildiği Değerler

##### PolGrp

| Poliçe Grubu | Kod |
| --- | --- |
| Bireysel Emeklilik | BES |
| Dask | DSK |
| Hayat | HAY |
| Kasko | KKO |
| Kaza | KZA |
| Mühendislik | MHE |
| Nakliyet | NAK |
| Sağlık | SAG |
| Trafik | TRF |
| Yangın | YNG |

##### TeklifDurumu

| Teklif Durumu | Kod |
| --- | --- |
| Ayarlanmadı | 0 |
| Teklif Verildi | 1 |
| Teklif Onaylandı | 2 |
| Teklif Onaylanmadı | 3 |
| Poliçe Yapıldı | 4 |

##### FyrlkDrm

| Yürürlük Durumu | Kod |
| --- | --- |
| Yürürlükte | 0 ve 1 |
| Yürürlükte Değil | 2 ve 3 |

##### fPolTek

| Poliçe Sınıfı | Kod |
| --- | --- |
| Ayarlanmadı | 0 |
| Poliçe | 1 |
| Teklif | 2 |
| Potansiyel  | 3 |
| Teklif Öncesi | 4 |

##### FfrkTt

| Tecdit Tipi | Kod |
| --- | --- |
|   |   |

Bu alan kullanılmamaktdır.

##### FpolTip

| Poliçe Tipi | Kod |
| --- | --- |
|   |   |

Bütün acentelerde kullanılmamakta, acenteniz kullanıyorsa acentenizden bilgi alabilirsiniz.

##### Cinsiyet

| Cinsiyet | Kod |
| --- | --- |
| Erkek | E |
| Kadın | K |
| Bilinmiyor |   |

##### Sigortalı Yakınlık

| Yakınlık | Kod |
| --- | --- |
| Kendisi | 1 |
| Eşi | 2 |
| Çocuğu | 3 |
| Bilinmiyor |   |

##### fKisiFirma

| Kişi / Firma | Kod |
| --- | --- |
| Şahıs | 1 |
| Firma | 2 |
| Bilinmiyor | 3 |

##### fMedDrm

| Kişi / Firma | Kod |
| --- | --- |
| Ayrı | 1 |
| Bekar | 2 |
| Dul | 3 |
| Evli | 4 |
| Bilinmiyor | 5 |

##### fMusTip

| Müşteri Tipi | Kod |
| --- | --- |
| Müşteri | 1 |
| TaliAcente | 2 |
| Satıcı | 3 |
| Sorumlu | 4 |
| Tahsildar | 5 |
| PoliceKesen | 6 |
| Bolge | 7 |
| Grup | 8 |
| Diger | 9 |

##### fBesOdeSek
<table>
<tr><th>Bes Ödeme Şekli</th><th>Kod</th></tr>
<tr><td>Yıllık</td><td>1</td></tr>
<tr><td>Altı Aylık</td><td>2</td></tr>
<tr><td>Üç Aylık</td><td>3</td></tr>
<tr><td>Aylık</td><td>4</td></tr>
</table>