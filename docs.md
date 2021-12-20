# Başlayalım

> Bu tutorial `2.4` sürümü için hazırlanmıştır.
---

## ArtChitecture

### Entity Oluşturma
> Entity nesneleri için bizim veritabanında tutacağımız modellerin C# için hazırlanmış karşılıkları diyebiliriz.
> Bu sayede veritabanında tuttuğumuz bir tabloyu sadece tek bir entity için modelleyip zahmetsiz kullanabiliriz.
> İlk entity nesnemizi oluşturalım
> Solution içinde bulunan `Entities` projesinin içindeki `Concrete` klasörüne sağ tıklayıp `Add->New Item` seçeneğini seçelim.
> Açılan sekmede `class` seçip nesnemize `Animal` ismini verip `Add` butonuna basalım
> ![Create Entity](tutorial-images/artchitecture-create-entity-class.png) 
>
> Açılan sayfaya aşağıdaki kod bloğunu ekleyin
> ```cs
> using Core.Entities.Abstract;
>
> namespace Entities.Concrete
> {
>    public class Animal : IEntity
>    {
>        public int Id { get; set; }
>        public string Name { get; set; }
>        public string OwnerName { get; set; }
>    }
> }
> ```
> Tebrikler ilk Entity sınıfınızı oluşturdunuz. 

---

### Entity için Veritabanı Modellemesi
> Şimdi oluşturduğumuz `Animal` nesnesi için veritabanı konfigürasyonlarını yapalım.
> 
> İlk önce `DataAccess` projesi içinde bulunan `ProjectDbContext.cs` dosyasını açalım
> ![ProjectDbContext](tutorial-images/artchitecture-projectdbcontext.png)
>
> Dosyanın içine aşağıdaki kod bloğunu resimdeki gibi ekleyelim.
> ```cs 
> public DbSet<Animal> Animals { get; set; }
> ```
> ![Add Entity to ProjectDbContext](tutorial-images/artchitecture-projectdbcontext-add-entity.png)
>
> Şimdi veritabanı tablomuzu oluşturalım
> 
> Bu aşamada karşımıza iki kavram çıkıyor
> - Code First
> - Db First
> 
> Hangisini seçeceğiniz size kalmış. Fakat `ArtChitecture` için tavsiye edilen yöntem `DB First` yöntemidir.

- #### Code First Yöntemi
    > Veritabanı tablosunu kod ile eklediğimiz yöntemdir.
    >
    > `Code First` yöntemini yapabilmek için `DbSet` yöntemi kullanarak `ProjectDbContext.cs` dosyasına Entity modelinizi eklemiş olmanız gerekmektedir.
    > 
    > `Package Manager Console` açılır Default Project `DataAccess` seçilir.
    > 
    > NOT: Başlangıç projenizin `WebAPI` olması gerekmektedir.
    > ```
    > add-migration Animal -Context ProjectDbContext -StartupProject WebAPI -Project DataAccess
    > ```
    > Yukarıdaki kodu `Package Manager Console` üzerinde çalıştırınız.
    > 
    > Migration oluştuktan sonra önünüze ilgili migration sınıfı gelecektir.
    > ![Example Migration](tutorial-images/artchitecture-example-migration.png)
    > 
    > NOT: Bu sınıfı kontrol etmeden veritabanını güncellemeyin !
    > 
    > Eğer herhangi bir hata almadıysanız ve migration istediğiniz gibi oluştuysa
    > ```
    > Update-Database -Context ProjectDbContext -StartupProject WebAPI -Project DataAccess
    > ```
    > Yukarıdaki kodu tekrar `Package Manager Console` üzerinde çalıştırınız.
    >
    > Hata almadıysanız tablonuzun başarıyla oluşmuş olması gerekmektedir.
    >
    >`NOT:` Bu yöntem sadece `Microsoft SQL Server` için test edilmiştir. Diğer veritabanlarında hata alabilirsiniz.

- #### Db First Yöntemi
    > Veritabanı tablosunu el ile eklediğimiz yöntemdir.
    >
    > Bu yöntemde veritabanı tablosunu kendiniz bir script yardımıyla veya başka bir araç yardımıyla oluşturabilirsiniz.
    >
    > `Code First` yöntemine göre daha sağlıklıdır.
    >
    > `ArtChitecture` için tavsiye edilen yöntemdir.

---

### DAL (Data Access Layer Object) Oluşturma
> `Dal` nesneleri bizim veritabanı işlemlerini tek bir merkezden yönetmemizi sağlayan nesnelerdir.
> 
> Bir `Dal` nesnesi oluşturalım 
>
> `Dal` nesnesi için solution içinde bulunan `DataAccess` projesi içindeki `Abstract` klasörüne sağ tıklayıp `Add->New Item` seçeneğini seçelim 
> Açılan ekranda `interface` seçip isim olarak `IAnimalDal` verip `Add` butonuna basalım.
> ![Create Dal](tutorial-images/artchitecture-create-dal.png)
> 
> Daha sonra açılan dosyaya aşağıdaki kod bloğunu ekleyelim
> ```cs
> using Core.DataAccess;
> using Entities.Concrete;
>
> namespace DataAccess.Abstract
> {
>    public interface IAnimalDal : IEntityRepository<Animal>
>    {
>    }
> }
> ```
> Bu işlemden sonra solution içinde bulunan `DataAccess` projesi içindeki `Concrete/EntityFramework` klasörüne sağ tıklayıp `Add->New Item` seçeneğini seçelim 
> Açılan ekranda `class` seçip isim olarak `EfAnimalDal` verip `Add` butonuna basalım.
> ![Create Dal](tutorial-images/artchitecture-create-efdal.png)
> 
> Daha sonra açılan dosyaya aşağıdaki kod bloğunu ekleyelim
> ```cs
> using Core.DataAccess.EntityFramework;
> using DataAccess.Abstract;
> using Entities.Concrete;
>
> namespace DataAccess.Concrete.EntityFramework
> {
>    public class EfAnimalDal : EfEntityRepositoryBase<Animal, ProjectDbContext>, IAnimalDal
>    {
>    }
> }
> ```
> Tebrikler `Animal` nesnesi için bir `Dal` nesnesi oluşturdunuz. 

---

### Service Oluşturma

### Service Özelleştirme

### Dal ve Service'i Birlikte Kullanma

---

### Controller Oluşturma

### Controller Özelleştirme

### Service ve Controller'ı Birlikte Kullanma 

---

### Bağımlılıkları Çözme

### İş Kuralları Yazma

### Validasyon Yazma

---

## ArtChitecture.Angular

### Entity Modelleme

### Service Oluşturma

### Component Oluşturma

### Componenti Sayfa Olarak Kullanma

### Sayfaları Korumak

### API ile Çalışma

---

## ArtChitecture.Flutter

### Entity Modelleme

### Service Oluşturma

### Component (Widget) Oluşturma

### API ile Çalışma

---

## İleri Seviye

### Aspects

#### Logging

#### Caching

#### Authorization

#### Transaction

#### Performance

#### Validation

#### Custom Aspect ve Interceptor

### Extensions ve Extension Yazma

### Helpers ve Helper Yazma

### IHttpContextAccessor

### IRequestUserService

### ITranslateContext ile Çoklu Dil Desteği

### Exceptions ve Errors

### ServiceTool ve Modules

### ITokenHelper ile Custom TokenHelper Oluşturma

### Startup.cs Konfigürasyonu

### SignalR Kullanarak Anlık Haberleşme

## Yayınlama
### ASP.Net Core
### Angular
### Flutter
