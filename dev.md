**
### 1. /Dev Dizini
**  
Bir cihazı bilgisayarınıza bağladığınızda, genellikle düzgün çalışabilmesi için bir cihaz sürücüsüne ihtiyaç duyar. Cihaz sürücüsüyle, cihaz dosyaları veya cihaz düğümleri aracılığıyla etkileşimde bulunabilirsiniz. Bu, normal dosyalar gibi görünen özel dosyalardır. Bu cihaz dosyaları tıpkı normal dosyalar gibi olduğu için, ls, cat gibi programlarla bunlarla etkileşimde bulunabilirsiniz. Bu cihaz dosyaları genellikle /dev dizininde saklanır. Sistemdeki /dev dizinini ls komutuyla inceleyin, sisteminizdeki çok sayıda cihaz dosyasını göreceksiniz.

```
$ ls /dev

```

Bu cihazlardan bazılarıyla daha önce etkileşimde bulundunuz, örneğin /dev/null. Hatırlayın, çıktıyı /dev/null'a gönderdiğimizde, çekirdek bu cihazın tüm girdimizi aldığını ve sadece onu yok ettiğini bilir, bu nedenle hiçbir şey geri döndürülmez.

Eskiden, sisteminize bir cihaz eklemek istediğinizde, /dev dizinine cihaz dosyasını ekler ve sonra muhtemelen onu unuturdunuz. Bunu birkaç kez tekrarladığınızda, bir sorunun nerede olduğunu görebilirsiniz. /dev dizini, çoktan yükseltilen, kullanılmayan vb. cihazların statik cihaz dosyalarıyla dağılmaya başlardı. Ayrıca, cihazlara atanan cihaz dosyaları, çekirdeğin onları bulduğu sıraya göre belirlenir. Yani her seferinde sisteminizi yeniden başlattığınızda, cihazlar keşfedildikleri zamana göre farklı cihaz dosyalarına sahip olabiliyordu.

Neyse ki artık bu yöntemi kullanmıyoruz, şimdi sistemde şu anda kullanılan cihazları dinamik olarak ekleyip kaldırmamızı sağlayan bir sistemimiz var ve bunu gelecek derslerde tartışacağız.

 - Fatma Öztürk tarafından çevrildi.
 - Sinem Aktaş tarafından kontrol edildi.


### 2. Cihaz Türleri

Cihazların nasıl yönetildiğinden bahsetmeden önce, bazı cihazlara bakalım.


    $ ls -l /dev
    
    brw-rw----   1 root disk      8,   0 Dec 20 20:13 sda
    
    crw-rw-rw-   1 root root      1,   3 Dec 20 20:13 null
    
    srw-rw-rw-   1 root root           0 Dec 20 20:13 log
    
    prw-r--r--   1 root root           0 Dec 20 20:13 fdata

Sütunlar, soldan sağa şu şekilde sıralanır:

-   İzinler
-   Sahip
-   Grup
-   Ana Cihaz Numarası
-   Minör Cihaz Numarası
-   Zaman Damgası
-   Cihaz Adı

Unutmayın, ls komutunda her satırdaki ilk bit, dosya türünü gösterir. Cihaz dosyaları şu şekilde tanımlanır:

-   c - karakter
-   b - blok
-   p - boru
-   s - soket

**Karakter Cihazı**

Bu cihazlar veri aktarır, ancak her seferinde bir karakter aktarırlar. Karakter cihazları olarak birçok sanal cihaz (/dev/null) göreceksiniz, bu cihazlar aslında makineye fiziksel olarak bağlı değillerdir, ancak işletim sistemine daha fazla işlevsellik sağlarlar.

**Blok Cihazı**

Bu cihazlar veri aktarır, ancak büyük sabit boyutlarda bloklar halinde. Verileri bloklar halinde kullanan cihazları, örneğin sabit diskler, dosya sistemleri gibi, blok cihazları olarak görürsünüz.

**Boru Cihazı**

Adlandırılmış borular, iki ya da daha fazla işlemin birbirleriyle iletişim kurmasını sağlar. Bunlar karakter cihazlarına benzer, ancak çıktının bir cihaza gönderilmesi yerine başka bir işleme gönderilmesini sağlar.

**Soket Cihazı**

Soket cihazları, süreçler arasında iletişimi kolaylaştırır, boru cihazlarına benzer, ancak birden çok işlemle aynı anda iletişim kurabilirler.

**Cihaz Karakterizasyonu**

Cihazlar, iki numara kullanılarak karakterize edilir: ana cihaz numarası ve minör cihaz numarası. Bu numaraları yukarıdaki ls örneğinde görebilirsiniz, aralarındaki virgülle ayrılmışlardır. Örneğin, bir cihazın cihaz numaraları şu şekilde olsun: 8, 0:

Ana cihaz numarası, kullanılan cihaz sürücüsünü temsil eder, bu durumda 8, genellikle sd blok cihazları için ana numaradır. Minör numara, çekirdeğe bu sürücü sınıfındaki hangi benzersiz cihaz olduğunu söyler, bu durumda 0, ilk cihazı (a) temsil eder.

### Alıştırmalar

/dev dizininize bakın ve görebileceğiniz cihaz türlerini öğrenin.

### Quiz

ls -l komutunda karakter cihazları için kullanılan sembol nedir?

 - İrem Acar tarafından çevirildi.
 -  Şeyma Karadeniz tarafından kontrol
   edildi.

### 3. Cihaz İsimleri
İşte karşılaşacağınız en yaygın cihaz adları:

*SCSI Aygıtları*
Makinenizde herhangi bir tür yığın depolama birimi varsa, büyük olasılıkla SCSI (“scuzzy” olarak telaffuz edilir) protokolünü kullanıyordur. SCSI, Küçük Bilgisayar Sistemi Arayüzü anlamına gelir ve diskler, yazıcılar, tarayıcılar ve diğer çevre birimleri ile sisteminiz arasındaki iletişimi sağlamak için kullanılan bir protokoldür. Modern sistemlerde kullanılmayan SCSI aygıtlarını duymuş olabilirsiniz, ancak Linux sistemlerimiz /dev içindeki sabit disk sürücüleri ile SCSI disklerine karşılık gelir. Bunlar sd (SCSI disk) ön eki ile temsil edilirler:

Ortak SCSI aygıt dosyaları:

 - /dev/sda - İlk sabit disk
 -  /dev/sdb -İkinci sabit disk
 -  /dev/sda3 - Birinci sabit diskteki üçüncü bölüm
 
 *Sözde Cihazlar*
 Daha önce de bahsettiğimiz gibi, sözde cihazlar sisteminize fiziksel olarak bağlı değildir, en yaygın sözde cihazlar karakter cihazlarıdır:
 - /dev/zero - tüm girdileri kabul eder ve atar, sürekli bir NULL (sıfır değer) bayt akışı üretir
 - /dev/null - tüm girdileri kabul eder ve atar, hiçbir çıktı üretmez
 - /dev/random - rastgele sayılar üretir
 
 *PATA Aygıtları*
 Bazen eski sistemlerde, sabit diskler hd ön ekiyle anılabilir:
 
 - /dev/hda - İlk sabit disk
 - /dev/hdd2 - 4. sabit diskteki ikinci bölüm
##
- Şeyma KARADENİZ tarafından çevirildi
-  İrem ACAR tarafından kontrol edildi.


### 4. sysfs
Sysfs, /dev dizininin yapamadığı cihazları daha iyi yönetmek amacıyla uzun zaman önce oluşturulmuştur. Sysfs, sanal bir dosya sistemidir ve genellikle /sys dizinine montelenir. /dev dizininde görebileceğimizden daha ayrıntılı bilgi sağlar.Hem /sys hem de /dev dizinleri çok benzer gibi görünüyor ve bazı açılardan gerçekten de öyledir, ancak önemli farklar da vardır. Temelde, /dev dizini basittir, diğer programların cihazlara doğrudan erişmesine izin verirken, /sys dosya sistemi cihaz hakkında bilgi görüntülemek ve cihazı yönetmek için kullanılır.
/sys dosya sistemi, temelde sisteminizdeki tüm cihazlara ait bilgileri içerir, örneğin üretici ve model, cihazın takıldığı yer, cihazın durumu, cihazların hiyerarşisi ve daha fazlası. Burada gördüğünüz dosyalar cihaz düğümleri değildir, bu nedenle /sys dizininden cihazlarla doğrudan etkileşime girmezsiniz, daha çok cihazları yönetiyorsunuz.

/sys dizininin içeriğine bir göz atın:

    pete@icebox:~$ ls /sys/block/sda
      
    alignment_offset  discard_alignment  holders   removable  sda6       trace
      
    bdi               events             inflight  ro         size       uevent
      
    capability        events_async       power     sda1       slaves
      
    dev               events_poll_msecs  queue     sda2       stat
      
    device            ext_range          range     sda5       subsystem

- Fatma Öztürk tarafından çevrildi.
 - Sinem Aktaş tarafından kontrol edildi.

### 5. Udev 
Eskiden ve aslında bugün, gerçekten isterseniz, cihaz düğümleri oluşturmak için şu gibi bir komut kullanırdınız:

    $ mknod /dev/sdb1 b 8 3

Bu komut, bir cihaz düğümü olan /dev/sdb1 oluşturur ve bunu bir blok cihazı (b) yapar, ana numara 8 ve yan numara 3 olur .Bir cihazı kaldırmak için, /dev dizinindeki cihaz dosyasını basitçe `rm` komutuyla silebilirsiniz.

Neyse ki, artık bunu yapmamıza gerek yok çünkü `udev` var. Udev sistemi, cihazların bağlı olup olmadığına göre cihaz dosyalarını dinamik olarak oluşturur ve kaldırır. Sistemde çalışan bir udevd daemon'u vardır ve bu daemon, çekirdekten gelen cihazlarla ilgili mesajları dinler. Udevd, bu bilgileri analiz eder ve veriyi, /etc/udev/rules.d dizininde belirtilen kurallarla eşleştirir. Bu kurallara bağlı olarak, cihazlar için muhtemelen cihaz düğümleri ve sembolik bağlantılar oluşturur. Kendi udev kurallarınızı yazabilirsiniz, ancak bu dersin kapsamı dışında kalmaktadır. Neyse ki, sisteminiz zaten birçok udev kuralı ile birlikte gelir, bu yüzden kendi kurallarınızı yazmanız hiç gerekmeyebilir.

Ayrıca udev veritabanını ve sysfs'i udevadm komutuyla görüntüleyebilirsiniz. Bu araç çok kullanışlıdır, ancak bazen oldukça karmaşık hale gelebilir. Bir cihazla ilgili bilgileri görüntülemek için basit bir komut şu şekilde olacaktır:

    $ udevadm info --query=all --name=/dev/sda
   
  - Gülsinem Aktaş tarafından çeviri yapıldı.
  - Fatma Öztürk taradından kontrol edildi.



### 6. lsusb, lspci, lssci
Tıpkı dosya ve dizinleri listelemek için ls komutunu kullandığımız gibi, cihazlar hakkındaki bilgileri listeleyen benzer araçları da kullanabiliriz.

*USB Aygıtlarını Listeleme*
$ lsusb
*PCI Aygıtlarını Listeleme*
$ lspci
*SCSI Aygıtlarını Listeleme*
$ lsscsi

 - Şeyma KARADENİZ tarafından çevirildi
 -  İrem ACAR tarafından kontrol edildi.


### 7. dd Komutu

dd aracı, veri dönüştürme ve kopyalama için çok faydalıdır. Bir dosyadan veya veri akışından giriş okur ve bunu bir dosyaya veya veri akışına yazar.

Aşağıdaki komutu göz önünde bulundurun:

    $ dd if=/home/pete/backup.img of=/dev/sdb bs=1024
Bu komut, backup.img dosyasının içeriğini /dev/sdb'ye kopyalar. Veriyi 1024 baytlık bloklar halinde kopyalar ve kopyalanacak veri kalmayana kadar devam eder.

 - if=file - Girdi dosyası, standart girdi yerine bir dosyadan okunur
 - of=file - Çıktı dosyası, standart çıktı yerine bir dosyaya yazar
 - bs=bytes - Blok boyutu, bir seferde bu kadar baytlık veriyi okur ve
   yazar. Boyutu kilobayt için k, megabayt için m vb. ile göstererek
   farklı boyut ölçümleri kullanabilirsiniz, bu nedenle 1024 bayt 1k'dır
 - count=number - Kopyalanacak blok sayısı.

    

Bazı dd komutlarında count seçeneği kullanılır. Genellikle, dd komutuyla 1 megabaytlık bir dosyayı kopyalamak isterseniz, bu dosyanın kopyalanması tamamlandığında 1 megabayt olarak görünmesini istersiniz. Şöyle bir komut çalıştırdığınızı varsayalım:

    $ dd if=/home/pete/backup.img of=/dev/sdb bs=1M count=2
Backup.img dosyamız 10M, ancak bu komutta 1M'yi 2 kez kopyalayacağımızı söylüyoruz, yani yalnızca 2M kopyalanacak ve bu da verimizin eksik olmasına neden olacak. Count, birçok durumda faydalı olabilir, ancak yalnızca veri kopyalıyorsanız, count ve hatta bs seçeneklerini göz ardı edebilirsiniz. Veri transferlerinizi gerçekten optimize etmek istiyorsanız, bu seçenekleri kullanmaya başlamalısınız.

dd çok güçlü bir araçtır, herhangi bir şeyi yedeklemek, tüm sabit diskleri yedeklemek, disk görüntülerini geri yüklemek ve daha fazlası için kullanılabilir. Ancak dikkatli olun, bu güçlü araç, ne yaptığınızdan emin değilseniz bir bedel ödetebilir.

### Alıştırmalar

dd komutunu kullanarak sürücünüzün bir yedeğini alın ve çıktıyı bir .img dosyasına yönlendirin.

### Quiz

dd komutunun blok boyutu için kullanılan seçeneği nedir?

 - İrem Acar tarafından çevirildi.
 - Fatma Öztürk tarafından kontrol edildi.


