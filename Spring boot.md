# **Kurulum**

**![](ekler/Pasted%20image%2020240226193434.png)**
* **`https://start.spring.io`  adresinden  ilgili Dependenci leri ekledikten sonra  install edilen dosyayı  idea da açıyoruz** 

**![](ekler/Pasted%20image%2020240226193359.png)**

* **yeni Dependencies eklemek için  `mvnrepository.com` adresinden  ilgili versiyonu seçerek  `pom.xml `de `Dependenciy` kısmına ekleme yaptıktan sonra sağ üstte çıkan `load mavan changes` e tıklayarak sync etmek gerekir.**

**![](ekler/Pasted%20image%2020240226193535.png)**
* **spring in default port u `8080` dir bunu değiştirmek için `application.properties` de `server.port` bilgisini belirtebiliriz**

**![](ekler/Pasted%20image%2020240226193607.png)**

---
# Temel Bilgiler
## **Controllers**

* **bir class ın `controller` olduğunu göstermek için `@RestController` `annatotion` kullanılır**
* Burada `endpoint` yönetimlerini yaparız
* mimarinin daha temiz olması için `controller` isminde yeni bir paket oluştururuz
* genelde `isimcontroller` şeklinde isimlendirilirler

## **Annotations**
* Bir clasın `controler` olduğunu spring e belirtmek  için `@RestController`  annotation u kullanılır
- **`@RequestMapping("url")` ile hangi url e cevap vereceğin belirtebiliriz**


```java
@GetMapping  
public String a(@RequestParam("abc") String name){  
    return  "Merhaba" + name;  
}

```

- `@GetMapping` ile gelen `GET` isteğini karşılarız 

![](ekler/Pasted%20image%2020240227020005.png)


- `@RequestParam` ile  hangi parametre ile isteği alacağımızı belirtebiliriz

- `@GetMapping("get3")` şeklinde `@GetMapping()` içine bir değer eklediğimizde path e eklenir ve yeni path i okur
```java

@GetMapping("{page}")  
public String b(@PathVariable String page) {  
    return "Get Request " + page;  
}

```
- `@GetMapping("{page}")`  bu şekilde de dinamik bir yapı ile path i okur
![](ekler/Pasted%20image%2020240227021548.png)


- body den gelen verileri almak ve göndermek için `class` kullanılır
- `@RequestBody` ile gelen variable veya arraylare erişebilirz
```java
@PostMapping()  
public Response b(@RequestBody Request request){  
    Response response = new Response();  
    response.setGivenNumber(request.getNumber());  
    return response;  
}
```

`Response.java`

```java
package com.turkcell.spring.starter.controller;  
  
public class Response {  
  
    int givenNumber;  
  
    public int getNumber() {  
        return givenNumber;  
    }  
  
    public void setGivenNumber(int number) {  
        this.givenNumber = number;  
    }  
}
```

![](ekler/Pasted%20image%2020240227022821.png)


# Web Api

![](ekler/Pasted%20image%2020240229123332.png)
- Yeni bir proje başlattığımızda modellerimizi tanıtmakla başlarız buna `Entity` denir    

![](ekler/Pasted%20image%2020240229124436.png)
- server response dönmeden önce bir çok işlem yapılır bunlardan bazıları `validation`,`Bussiness Rules`,`cache` ....

## Swinger

[swinger maven](https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-starter-webmvc-ui/2.3.0) Dependencies si eklendikten sonra 
![](ekler/Pasted%20image%2020240228184427.png)
`http://localhost:8080/swagger-ui/index.html` bu url üzerinden  kullanılabilir


# Verinin saklanması
### sistemin mimarisi 
- N-layered

`entities` :model tanımları 
`controller` : endpoint tanımı
`DataAcces` (repositories) :  Veri erişimi
`Service` (Business) : validation ve business roles
`core` : çekirdek kodlar 

![](ekler/Pasted%20image%2020240229130952.png)

![](ekler/Pasted%20image%2020240229131608.png)

- `Productcontroller.java`'ın `ProductService.java` ı çağırabliyor olması gerekiyor. 
- buda ona bağımlı olduğunu gösterir
- Dependency Injection


> [!NOTE] Ioc
> `ProductService` i `prodoctServiceImpl` e new lemek yanlış olur çünkü bizim olabildiğince soyut olan class dan değişkenlik kurmamız gerkiyor
> Olası bir değişkenlik durumunda güncellemeler kolay yapılabilmeli
> `productServiceImlp` de  `repository` lere bağımlı olacak ve bir güncelleme yapabilmek için çok fazla değişiklik yapmak gerekecek
> Bu noktada bağımlılığımızı classın başında `CTOR` kullanarak çözeceğiz.



> [!NOTE] Title
> Burada `CTOR` kullanımında yine de ben `productService` i newlemem bunu spring frameworkün kendi altyapısı newler.



> [!NOTE] Spring Dependency leri nasıl çözer
> Spring Ioc- dependency Container
> Uygulama bağlarken `ProductService` isimli dosyamın karşılığı `ProductServiceImpl` dir 
> Benzer bir şekilde  `ProductRepository` isimli dosyamın karşılığı `InMemorProductRepositoryImpl` dir
> 

![](ekler/Pasted%20image%2020240229183252.png)

- `ProductController` spring IOC ye ben `ProductService` bağımlılığı yapmak istiyorum der 
- Bunu IOC ye bu bağımlılığın tanımlı olup olmadığını sorduğunda IOC karşılığında bağımlı olduğu `ProductServiceImpl` e yönlendirir. Böylece bizim newleme yapmamıza gerek kalmaz 
- Bu bağımlılığı IOC ye eklemek için Spring de @Service anotation u  service classının başına eklendiğinde IOC ye eklenmiş olur




### Solid
Single Responsibiliry Principle



# Veri Tabanları

#### Veri tabanı tasarımları


> [!NOTE] ER diagram
> entity relation diagram


#### veritabanı normalizasyonu 

- İlişkisel bir hale getirerek  hata yapma olasılığını azaltır
- String değerleri  farklı bir tabloda tutarak id bilgisi ile bağlantı kurulur

![](ekler/Pasted%20image%2020240301183537.png)

![](ekler/Pasted%20image%2020240301185748.png)
- MTM  bağlantısı kurmak için 3. bir tabloya ihtiyaç duyulur.

### PostgreSql admin 
- db oluşturma 
- Pk belirtme
- one-to-many  ForeignKey ekleme
- backup ile dışarı aktarma
- İmport etme

# SQL

> [!NOTE]
>  SQL - Structured Query Language
> 
> - DDL - DML
> - Data Definition Language -> Verinin tanımını işleyen sql queryleri.
> - Data Manipulation Language -> Verinini kendisini işleyen sql queryleri.

```sql


-- SELECT [kolonlar] from {tablo}
SELECT * FROM products;

SELECT id,name from products;

Select * from products WHERE unitprice > 200 or stock > 5

Select * from products WHERE name='Laptop'

Select * from products ORDER BY stock ASC -- default ASC yazma gereği yok.
-- Ascending, Descending
Select * from products ORDER BY stock DESC

Select * from products 
Where stock > 0 
ORDER BY stock DESC
-- la
Select * from products WHERE name='Laptop'
-- LIKE
-- % -> Sağ ve sol tarafa eklendiğinde o kısımda kaç adet ve hangi karakter olduğunu umursamaz.

-- L Harfi ile başlayan ürünler
Select * from products WHERE name LIKE 'L%'

-- p harfi ile biten ürünler
Select * from products WHERE name LIKE '%p'

-- Mo ile başlayan e ile biten ürünler
Select * from products WHERE name LIKE 'Mo%e'

-- _
-- _ -> Sağ ve sol tarafa eklendiğinde o kısımda hangi karakter olduğunu umursamaz.
Select * from products WHERE name LIKE 'Mo__e'

Select * from products WHERE name LIKE '_____'



```

#### Advanced Search ElasticSearch

 #### ESCAPE CHARACTER
```sql
 Select * from products WHERE name LIKE '%\%%' ESCAPE '\';
```

```sql
Select * from products WHERE unitprice > 50 and unitprice < 500

Select * from products WHERE unitprice BETWEEN 51 AND 499

Select * from products WHERE LOWER(name) BETWEEN 'a' AND 'f%'

```


#### Aggregate Function
```sql
-- En düşük fiyat nedir?
Select min(unitprice) from products
-- En yüksek fiyat nedir?
Select max(unitprice) from products
-- Ortalama fiyat nedir?
SELECT avg(unitprice) as "Ortalama Ürün Fiyatı" from products
-- Kaç adet ürünüm var?
Select COUNT(*) as "Ürün Sayısı" from products

SELECT COUNT(*) as "Ürün Sayısı", 
avg(unitprice) as "Ortalama Ürün Fiyatı", 
max(unitprice) as "En Pahalı Fiyat",
min(unitprice) as "En Ucuz Fiyat"
from products

Select * from products WHERE CHAR_LENGTH(name) > 10

```

#### İç içe sorgular
- Inner Queries
```sql
Select * from products where unitprice = (  Select min(unitprice) from products  )

Select * from products where unitprice = (  Select max(unitprice) from products  )

Select * from products where unitprice > (  Select avg(unitprice) from products  )

Select * from products where unitprice < (  Select avg(unitprice) from products  )
```

#### JOİN

![](ekler/Pasted%20image%2020240304194602.png)

```sql


-- Update
-- Koşulu unutursak tüm tablo etkilenir!!
Update products
SET category_id=4, stock=70
WHERE id=18

-- Delete
Delete from products where id=19

----

-- JOIN
Select * from products 
INNER JOIN categories ON products.category_id=categories.id --fk pk birleşimini konfigüre
-- 
Select products.name,categories.name from products 
INNER JOIN categories ON products.category_id=categories.id
--
Select p.name, c.name, p.unitprice, c.id from products p
INNER JOIN categories c ON p.category_id=c.id
--
Select * from users u
JOIN carts c ON u.id=c.user_id
JOIN products_carts pc ON c.id=pc.cart_id
--
Select * from carts c
JOIN products_carts pc on c.id=pc.cart_id
--
Select * from carts c
LEFT JOIN products_carts pc on c.id=pc.cart_id
WHERE pc.id IS NULL
--
Select * from carts c
RIGHT JOIN products_carts pc on c.id=pc.cart_id
--
Select * from carts c
FULL OUTER JOIN products_carts pc on c.id=pc.cart_id
--
-- Group By -> 
-- 15 adet soru-cevap querysi.
-- Hangi ürün kaç adet sattı?
-- Cevap

```












# Spring Framework ve DataBase Connection

`pom.xml`

```java
<dependency>  
    <groupId>org.postgresql</groupId>  
    <artifactId>postgresql</artifactId>  
    <scope>runtime</scope>  
</dependency>  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-jpa</artifactId>  
</dependency>
```

- `postgresql` ve `jpa` dependency lerini ekledik 

`application.properties`

```java
#server.port=1010
spring.datasource.url=jdbc:postgresql://localhost:5432/turkcelldb
spring.datasource.username=postgres
spring.datasource.password=test

```
- `application.properties` dosyasında postgresql e bağlantısını yaptık

```java
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
#... jpa konfigürasyonu

```

- spring jpa bağlantısı için yine `application.properties` kısmına ekliyoruz.
- 

### Category 

`Category.java` Entity

```java

package com.turkcell.spring.starter.entities;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;


@Table(name = "categories")
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Category
{
    @Column(name="id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Id
    private int id;

    @Column(name="name")
    private String name;
}

```

- entity dediğimiz şey aslında temelinde  veritabanı tablolarının java class ı haline çevrilmesidir.
- bir classın tablo olarak tanımlanması için öncelikle  jpa paketinde  `@Table` anatosyonu ile  ilgili class vermemiz ve  tablo ismini `@Table(name = "categories")` olarak belirtmemiz gerekiyor
- daha sonrasında `@Column` anatosyonu ile vertabanındaki tabloları belirtiyorum
- `@Entity` ile bu class ın bir database class ı olduğunu belirtiyorum
- `lambok` kütüphanesinden gelen `@Setter` ve `@Getter` ile getter ve setter i hızlı bir şekilde oluşturabiliz
- ikisini birlikte oluşturmak istersen `@Data` ile ikisini birlikte ekleyebiliriz

- `id` nin tablonun `pk` sı  olduğunun anlaşılabilmesi için `@id` kullanılır 
- `identity` olduğunu belirtmek için `@GenerationType` kullanılır

- `@AILArgsConstructor` `lombok` kullanarak tüm `CTOR` ları oluşturmamızı sağlar

`CategoryRepository`  abstracts

```java
package com.turkcell.spring.starter.repositories.abstracts;

import com.turkcell.spring.starter.entities.Category;
import org.springframework.data.jpa.repository.JpaRepository;

public interface CategoryRepository extends JpaRepository<Category, Integer>
{
}
```

`CategoryService.java`  abstracts
```java

package com.turkcell.spring.starter.services.abstracts;

import com.turkcell.spring.starter.entities.Category;

import java.util.List;

public interface CategoryService {
    List<Category> getAll();
}

```


`CategoryServiceImpI.java`  concretes

```java

package com.turkcell.spring.starter.services.concretes;

import com.turkcell.spring.starter.entities.Category;
import com.turkcell.spring.starter.repositories.abstracts.CategoryRepository;
import com.turkcell.spring.starter.services.abstracts.CategoryService;
import lombok.AllArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;

@AllArgsConstructor
@Service
public class CategoryServiceImpl implements CategoryService {
    private final CategoryRepository categoryRepository;

    @Override
    public List<Category> getAll() {
        return categoryRepository.findAll();
    }
}
```



`CategoriesController`

```java
package com.turkcell.spring.starter.controllers;

import com.turkcell.spring.starter.entities.Category;
import com.turkcell.spring.starter.services.abstracts.CategoryService;
import lombok.AllArgsConstructor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("/api/categories")
@AllArgsConstructor
public class CategoriesController {

    private final CategoryService categoryService;

    @GetMapping
    public List<Category> getAll(){
        return categoryService.getAll();
    }
}

```

### Products

`Products.java`


- Fk ler direk `@column` olarak eklenmez  one-to-many ilişikiyi kurgulamak için için başına  `@one-to-many` anatasyonu gelir
- içinde bulunduğumuz class hangi kısımda bulunuyorsa anatasyonu ona göre yaparız
- many kısmı `@ManyToOne` one kısmın `@OneToMany`
- `@MTO` kısmına `@JoinColumn(name: " category_id l' )` `@JoinColumn` eklenmesi gerekir
- `@OTM` kısmına `@0neToMany(mappedBy "category")`  `mappedBy` gelir


mesela product tablosunda yeni bir field eklendi ve bu değer not Null. Eski verilerde olmayan veriler için çıkan hatayı gidermenin tek opsiyonu defult ayarlamak mı ?



### DTO  data transfer object

- benim kullanıcıları listelediğim bir alanın içerisindeki `password` gibi tüm bilgileri  `Controller` dan  veritabanı modelini iletmesini istemeyiz
- `DTO` ile hangi istekte hangi fieldlerin kullanılacağı belirlenir.

- `DTO` ları `services` içerisinde tutarım 
- Aynı zamanda `DTO` dediğimiz şey bir `entitiy` için birden fazla oluşturulabilir. Listelerken eklerken güncellerken gibi.
- Bu sebep `services/DTO/product` java package i oluşturulur.


`services / dtos / product / ProductForAddDtojava`

```java

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class ProductForAddDto {
    private String name;
    private double unitPrice;
    private long stock;
    private int categoryId;
}

```
- add işlemi için kullanacağım fieldleri belirledim.

`ProductsController.java`

```java

@RestController
@RequestMapping("/api/products")
public class ProductsController
{
    // Dependency Injection
    // @Autowired
    private ProductService productService;

    public ProductsController(ProductService productService) {
        this.productService = productService;
    }

    // DTO => Data Transfer Object
    @PostMapping
    public void add(@RequestBody ProductForAddDto productForAddDto)
    {
        productService.add(productForAddDto);
    }
    @GetMapping
    public List<Product> get() {
        return productService.getAll();
    }
}
```
- `add` methodu içerisindeki `Product`  değişkenini yerine oluşturmuş  olduğum `ProductForAddDto` u verdim 
- Bu noktoda yazmış olduğumuz services lerde de bir güncelleme gerekiyor.


`ProductService.java`

- ilk önce somutta güncellemeyi yaparak başlıyorum
```java

public interface ProductService
{
    void add(ProductForAddDto productForAddDto);
    List<Product> getAll();
}

```

- daha sonra soyutta
```java


@Service
public class ProductServiceImpl implements ProductService
{
    private ProductRepository productRepository;

    public ProductServiceImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Override
    public void add(ProductForAddDto productForAddDto) {
        if(productForAddDto.getUnitPrice() < 0)
            throw new RuntimeException("Ürün fiyatı 0'dan küçük olamaz.");

        // TODO: Check from db
        Category category = new Category();
        category.setId(productForAddDto.getCategoryId());

        // Mapping -> Manual
        // TODO: Auto Mapping
        Product product = new Product();
        product.setName(productForAddDto.getName());
        product.setStock(productForAddDto.getStock());
        product.setUnitPrice(productForAddDto.getUnitPrice());
        product.setCategory(category);
        productRepository.save(product);
    }

    @Override
    public List<Product> getAll() {
        return productRepository.findAll();
    }
}

```

* burada daha sonra bakılması gerekenler `Auto Mapping` ve  category eklerken eklenmek istenen categorynin db de olup olmadığını `Check from db`
#### Request-Response pattern 
- her istege ve her cevaba özel birer bir nesne gibi davranarak her birine birer nesne ekleyeceğiz. 
- bu nedenle `DTO` ları `request` ve `response` olarak ikiye ayıracağız.
- isimlendirmeler de  `AddProductRequest` gibi yapılacak.
- Gerekli isim değişiklikleri `controller` ve `services` kısmında yapılır. 

`services / dtos / product / requests / AddProductRequestjava`

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class AddProductRequest {
    private String name;
    private double unitPrice;
    private long stock;
    private int categoryId;
}
```

`services / dtos / product / responses / ProductListResponse.java`

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class ProductListResponse {
    private int id;
    private String name;
    private String categoryName;
    private double unitPrice;
}

```

`ProductsControIIerjava`

```java
@RestController
@RequestMapping("/api/products")
public class ProductsController
{
    // Dependency Injection
    // @Autowired
    private ProductService productService;

    public ProductsController(ProductService productService) {
        this.productService = productService;
    }

    // DTO => Data Transfer Object
    @PostMapping
    public void add(@RequestBody AddProductRequest request)
    {
        productService.add(request);
    }
    @GetMapping
    public List<ProductListResponse> get() {
        return productService.getAll();
    }
}

```


`Productservice.java`

```java
import java.util.List;

public interface ProductService
{
    void add(AddProductRequest request);
    List<ProductListResponse> getAll();
}

```

`ProductServiceImpl.java`

```java
@Service
public class ProductServiceImpl implements ProductService
{
    private ProductRepository productRepository;

    public ProductServiceImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Override
    public void add(AddProductRequest request) {
        if(request.getUnitPrice() < 0)
            throw new RuntimeException("Ürün fiyatı 0'dan küçük olamaz.");

        // TODO: Check from db
        Category category = new Category();
        category.setId(request.getCategoryId());

        // Mapping -> Manual
        // TODO: Auto Mapping
        Product product = new Product();
        product.setName(request.getName());
        product.setStock(request.getStock());
        product.setUnitPrice(request.getUnitPrice());
        product.setCategory(category);
        productRepository.save(product);
    }

    @Override
    public List<ProductListResponse> getAll() {
        List<Product> products = productRepository.findAll();
        List<ProductListResponse> response = new ArrayList<>();

        // Beginner Level List Mapping
        for (Product product: products) {
            ProductListResponse dto = new ProductListResponse(
                    product.getId(),
                    product.getName(),
                    product.getCategory().getName(),
                    product.getUnitPrice());
            response.add(dto);
        }
        // Request - Response (Reply) Pattern
        return response;
    }
}

```

### jpql & jpa  | Derived Query Methods

- jpa nın defoult methodları üzerine benim custom methodlarımı eklemek için bu yapıyı kullanırım.

- [jps query methods]( https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html)

`ProductRepository.java`

```java
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Integer>
{
    // Native Query => Saf SQL
    //@Query(value="Select * from products where LOWER(name) LIKE %:query% ", nativeQuery = true)

    // JPQL => JPA + SQL
    @Query(value = "Select p from Product p WHERE LOWER(p.name) LIKE %:query%")
    List<Product> search(String query);

    @Query(value = "Select new com.turkcell.spring.starter.services.dtos.product.responses.ProductListResponse" +
            "(p.id, p.name, c.name, p.unitPrice)" +
            " from Product p JOIN p.category c" +
            " WHERE lower(p.name) LIKE %:query%"
            )
    List<ProductListResponse> searchDto(String query);

    // Derived Query Methods
    // Oto hazırlanmış method isimlendirmeleri
    List<Product> findByUnitPriceGreaterThan(double unitPrice);
}

```

- `ProductListResponse` ın path ını vermemiz gerekiyor. 

> [!NOTE] mapping işlemi 
> - Mapping işlemini  service katmanında yapmak her zaman daha doğrudur. 
> - `Repository` de yapğabiliyor olmamız doğru olduğu anlamına gelmez
>- arama işlemimiz her zaman sabit bit `DTO` ile çalışmak zorunda değil. 
> - Arama işleminde db verisi geri dönüp `Services` de `DTO` ya çevirmek daha doğrudur. böylece arama işlemini birden fazla `service` işleminde kullanılabilir
>- Aynı zamanda `Derived Query Methods` ile birlikte `DTO` kullanımı olmamakta bunu yapabilmek için `DTO` ya çevirmeyi `Service` de kullanmak daha doğru olur.



**N architecture ?**

### Cross Cutting Concerns
- uygulamayı dikine bölen yapılar demektir. 

- Uygulamanın klasik çalışma mantığında işlemlerde araya giren ve kendi işlemlerini uygulayan yapılardır. 

> [!NOTE] Örnek
> Örnek olarak loglama cashlame validation authorization transaction yönetimi



![](ekler/Pasted%20image%2020240309142009.png)

![](ekler/Pasted%20image%2020240309142323.png)

- validasyonla başlarken spring de `spring-boot-starter-validation` 'ı dependency ler arasına ekliyorum.


#### validations 
`AddProductRequest`

![](ekler/Pasted%20image%2020240309143016.png)
- `@Notblank` ,`@Length` gibi bazı hazır validasynlar bulunmakta 

- [jakarta hazır validations](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/package-summary)

`ProductController`
![](ekler/Pasted%20image%2020240309143103.png)

- bu `DTO` u ilk kullandığım yere bir validation ekledim 

örnek hatalı post request
![](ekler/Pasted%20image%2020240309143245.png)

- mesajları değiştirmek için ilgili validasyon kuralına message belirtebilir.
- ileride bu mesajları farklı bir dosyadan alacağız.
![](ekler/Pasted%20image%2020240309143418.png)


#### Business Rules
`ProductServiceImpl`

![](ekler/Pasted%20image%2020240309144733.png)

- örnek post request
![](ekler/Pasted%20image%2020240309144826.png)


aynı bussines role u  birden fazla kez kullanabiliriz bunun için 

`ProductServiceImpl`

![](ekler/Pasted%20image%2020240309151332.png)

![](ekler/Pasted%20image%2020240309151243.png)



## Exceptions

![](ekler/Pasted%20image%2020240317110434.png)

- Core kısmı farklı projelerde direk kullanılabilcek bir yapıya sahiptir. 
- Genelde şirketlerin genel kurallarını içererir. 
- Bu kurallar yeni oluşturulacak uygulamalara direk entegre edilerek kullanılabilir. Çünkü `service` `repository` ya da `entitiy` ile direk bir bağlantısı bulunmamakta
### GlobalExceptionHandler

`../Core/exceptions`

![](ekler/Pasted%20image%2020240309152303.png)
- `@ExceptionHandLer` da hangi hata için hangi işlemi yapacağı belirtilir
- `@ResponseStatus` ile hangi `HttpStatus` u döneceği belirtilebilir.
![](ekler/Pasted%20image%2020240309152520.png)
- `RuntimeException` dönen methodda belirtilen mesajı almak için `handleRuntimeException` içerisinden  `exception` mesajı alınabilir


- kod içerisinde `breakpoint` ekleyerek debug modunda analiz edebiliriz
- breakpoint e düşmesi için `exception` a düşeş methodun çalıştırılması yani o method a bir istek gönderilmesi gerekecek
![](ekler/Pasted%20image%2020240309152740.png)

![](ekler/Pasted%20image%2020240309153035.png)

- bir exception type ı birden farklı noktada olabilir
- bu yüzden `types` isminde bir paket içerisinde  `BusinessException` oluşturabiliriz

`BusinessException`

![](ekler/Pasted%20image%2020240309153516.png)
``
`ProductServiceImpl`
![](ekler/Pasted%20image%2020240309153328.png)
- `ProductServiceImpl` içerisinde `RuntimeException` yerine `BusinessException` ı  kullanarak hatayı kullandığımız method a özelleştirdik

`GlobalExceptionHandler`

![](ekler/Pasted%20image%2020240309153347.png)


- `BusinessException` da ne döneceğini spesifikleştirdik

![](ekler/Pasted%20image%2020240309153717.png)
- ele alınmamış tüm hatalara dönülecek mesajı belirledik


## Multi Langugage

### multilang clean code refactoring

### mapstruct without lombok




# Docker

![](ekler/Pasted%20image%2020240313201232.png)


> [!NOTE] Docker Kullanım amacı
> Farklı projeler farklı versiyonlarla çalıştığını düşündüğümüz senaryoda, bilgisayarımıza tüm versiyonları kurmak yerine docker kullanıyoruz.
> yazılım ürünlerini birbirinden izole ve bağımsız container haline getirip o şekilde çalışır
> Örnegin kullandığımız uygulamaların her versiyonu ayrı olarak services olarak bilgisayarımıza eklenir.
> Böylece container yapısı ile projenin bağımlılıklarını çözümlemiş oluruz.

### Images

- Bilgisayarımıza postgresql kurduğumuzda bazı dosyalar kayıt edilir  kurduğumuz postgresql dosyaları Imagesdır
- service henüz çalışmaz

[DockerHub](https://hub.docker.com/)

herhangi bir `Image` i yüklemek için  
```
docker pull  paketismi:versiyonu

docker pull postgres:16.2-alpine
```

![](ekler/Pasted%20image%2020240313200918.png)
![](ekler/Pasted%20image%2020240313201012.png)

- şuan için yapmış olduğumuz  .exe dosyasını alıp kurulumu yaptık ama `service`i çalışmıyor 

### Container
- `service` in çalışması için container e ihtiyacımız var

![](ekler/Pasted%20image%2020240313201143.png)
- çalıştırmak için `images` den run yapılabilir.  
![](ekler/Pasted%20image%2020240313202111.png)
- windowstaki belirlediğimiz portun `wsl` de hangi porta eşit olduğunu belirtir
- `Environment variables`  kısmında `postgresql` için şifre gibi  değişkenler set edilir.
[Environment Variables for postgres](https://hub.docker.com/_/postgres)
- windows port u 5433 belirledik 

![](ekler/Pasted%20image%2020240313202357.png)
- PgAdmin den erişim için bu ayarlar kullanılabilir.

### Volumes

- kullanıcı verileri
- bir db ye veri eklediğimizde eklenen veriler volume de kayıt edilir

![](ekler/Pasted%20image%2020240313205231.png)
- `volume` ismi belirtilebilir 
- bir `container` e spesific bir `volume` belirlenmiş olur
- Bu şekilde `volume` oluşturulursa veriler kalıcı bir hale gelir ve `container` silinse bile `volumes` yani veriler silinmez.
- Böylece yeni bir `container` oluştururken  aynı isimle `volume` verilirse  veriler kaldığı yerden devam eder 


### Kendi projemizi Docker İmajı haline getirmek.

- java uygulamaları  canlı ortama geçtiğinde .jar dosyaları ile çalışır.
- build etmek için java da `mvn clean package` ile build eder.
- ve `target` altında  `.jar` dosyası verir 
- `java -jar <dosyaismi>.jar` ile java nın kurulu olduğu her dosyayı çalıştırabiliriz.
- proje ana dizininde  `Dockerfile` dosyayı oluşturulur

`Dockerfile`

```Dockerfile
# Adım adım neler yapması gerekiyor?


# Sıfır bir linux makinada bu sistemi çalıştırmak istesem ne yapardım?

FROM maven:3.8.3-openjdk-17 as build
WORKDIR /app
COPY . /app/
# sol taraf windows - sağ linux
RUN mvn clean package
# 5-10 => Yazılımı build etme işlemi

# Build edilen dosyayı ayağa kaldırma.
FROM openjdk:17-jdk
COPY --from=build /app/target/spring.starter-0.0.1-SNAPSHOT.jar /app/myapp.jar
CMD java -jar /app/myapp.jar

```


- `Dockerfile` nın olduğu path de  `docker build turkcell-spring .`  ile  imaj oluşturulur. 


### Dockercompose 

docker-compose.yuml

- içerisinde birden fazla  kontainer vardır
- tek bir dosya ile birden fazla kontainer i bir arada çalıştırmaya yarar.

- En çok Microservislerde kullanılır tek bir dosya ile tüm `dependency` ler belirlenir 

``` docker
services:  
  discovery-server:  
    build: ./discoveryserver  
    ports:  
      - "8888:8888"  
    networks:  
      - common  
  gateway-server:  
    build: ./gatewayserver  
    ports:  
      - "8001:8001"  
    environment:  
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://discovery-server:8888/eureka/  
      - EUREKA_URL=http://discovery-server:8888  
    depends_on:  
      - discovery-server  
    networks:  
      - common  
  customer-service:  
    build: ./customerservice  
    ports:  
      - "8079:8083"  
    environment:  
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/customerdb  
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://discovery-server:8888/eureka/  
    depends_on:  
      - postgres  
      - discovery-server  
    networks:  
      - common  
  order-service:  
    build: ./orderservice  
    ports:  
      - "8077:8081"  
    environment:  
      - SPRING_DATA_MONGODB_URI=mongodb://root:root@mongodb:27018/orderservice?authSource=admin  
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://discovery-server:8888/eureka/  
    depends_on:  
      - mongodb  
      - discovery-server  
    networks:  
      - common  
  mongodb:  
    image: mongo  
    ports:  
      - "27018:27018"  
    environment:  
      - MONGO_INITDB_ROOT_USERNAME=root  
      - MONGO_INITDB_ROOT_PASSWORD=root  
    volumes:  
      - mongo-volume:/data/db  
    networks:  
      - common  
  postgres:  
    image: postgres  
    ports:  
      - "5433:5432"  
    environment:  
      - POSTGRES_PASSWORD=2144  
    volumes:  
      - postgresql-volume:/var/lib/postgresql/data  
    networks:  
      - common  
  
  
volumes:  
  postgresql-volume:  
    name: postgresql-volume  
  mongo-volume:  
    name: mongo-volume  
  
networks:  
  common:  
    driver: bridge
```


![](ekler/Pasted%20image%2020240315195918.png)

![](ekler/Pasted%20image%2020240315195921.png)
- birlikte çalıştırma kiçin bu komuş yazılır
- -d   `Deteach` demketir konsola çıktılarını verir ama bir konsol uygulması gibi çalışmaz

- `mvn clean package` ile  .jar dayası oluşturulur 
- bu dosyanın ismi  `Docker` dosyasına eklenir
- `docker build -t turkcell-orderservice . ` ile  `image` oluşturulabilir.

*  kodda herhangi bir güncelleme yaptıktan sonra tekrar 
`docker compose up --build -d` komutunu çalıştırmak gerekiyor.



## Microservices

## Spring Cloud
- mikroservislerin `distributed services` bazı kısayollarla işimizi kolaylaştıran bir alt kütüphane.  
- daha sonra tekrar `microservis` iletişimleri sırasında değinilecek. 
-  `# Servisler arasında iletişim`  -->

![](ekler/Pasted%20image%2020240328234757.png)

- Fİle> project structure > modules  kısmından  eklediğimiz her microservice i module  olarak `pom.xml` ini seçerek  belirtmemiz gerekiyor
![](ekler/Pasted%20image%2020240317112811.png)


- Tüm servisler belirlendikten sonra ilgili servislerin ilgili db lerle bağlantısı için  pom.xml de düzenlemeler yapılacak 
- ![](ekler/Pasted%20image%2020240317113553.png)
- her servisin farklı portta çalışması için `aplication.properties` dosyasında  `server.port=8081` şeklinde farklı portlar atayarak güncelleme yapılır.



## Advanced Architecture of Microservices

![](ekler/Pasted%20image%2020240421190029.png)

 - **`clients`**: Dış servislerle iletişim kurmak için kullanılan `client` sınıflarını içerir.
 - **`controllers`**: Uygulamanın HTTP isteklerini karşıladığı ve yönlendirdiği sınıflar bulunur. `Controllerlar`, dış dünyadan gelen isteklere cevap vermek için kullanılır.
 - **`core`**: Uygulamanın temel işlevselliğini içeren sınıflar burada yer alır. Bu alt dizinler genellikle yapılandırma (`configuration`), veritabanı nesneleri (`entities`), hata yönetimi (`exception`) ve servis katmanı (`service`) sınıflarını içerir.
 - **`entities`**: Veritabanı ile etkileşimde bulunan sınıflar, yani entity sınıflar burada yer alır.
 - **`repositories`**: `Spring Data JPA`'nın repository arayüzlerini içerir; bu arayüzler, veritabanı işlemleri için kullanılır.
 - **`services`**: İş mantığının uygulandığı sınıflar, bu klasör altında toplanır. İş mantığı genellikle `abstract` ve `concrete` sınıflar olarak ayrılır.
 - **`dtos`**: `Data Transfer Objects`, katmanlar arası veri taşımak için kullanılan nesnelerdir. Bu dizin, istek `request` ve yanıt `response` tiplerini içeren `DTO` sınıflarını barındırır.
 - **`mappers`**: Nesneler arası veri transferi için kullanılan ve genellikle model sınıfları ile `DTO` sınıfları arasındaki veri aktarımını sağlayan `mapper` sınıflarını içerir.
 - `**rules**`: İş kurallarını içeren sınıflar bu klasör altında yer alabilir.



# Mongo db
`pom.xml` e mongo db dependency yi ekledik

![](ekler/Pasted%20image%2020240328114351.png)

## mongo in docker

`docker pull mongo`
![](ekler/Pasted%20image%2020240326231719.png)

![](ekler/Pasted%20image%2020240326231954.png)
`27017` mongonun default portudur

[mongo db app](https://www.mongodb.com/products/tools/compass)


![](ekler/Pasted%20image%2020240328114952.png)
- `Collection Name` sql de denk gelen tablonun ismidir.

`aplication.properties`

![](ekler/Pasted%20image%2020240320200839.png)

- Nosql ile sql arasında tek fark entity tanımlaması sırasında olur
- MongoDB nosql çalışan bir db dir.
- json yapısı ile bilgileri tutar

## Entity deki farklar 
![](ekler/Pasted%20image%2020240320201205.png)
- id yi `String` olarak tanımlamak gerekir. 
- `@Document` anatosyonu alır
- `@Column` anatosyonu almaz çünkü sql deki gibi bir colum yapısı yok.

`OrderReposirory`

![](ekler/Pasted%20image%2020240328115329.png)

`jpaRepository` yerine `MongoRepository` ve id nin türünü belirterek `Repository`i bu şekilde oluşturduk.

## Query farklılıları.






# Servisler arasında iletişim

## SYCN iletişim 

![](ekler/Pasted%20image%2020240320202324.png)
- Spring içerisinde `HTTP` isteği atabilmemizi sağlayan bir kütüphane.
![](ekler/Pasted%20image%2020240320203353.png)
### WebFlux

![](ekler/Pasted%20image%2020240320202447.png)
altında  `WebClientConfig` oluşturulur.
![](ekler/Pasted%20image%2020240320202520.png)

- daha sonrasında service de 
![](ekler/Pasted%20image%2020240320203055.png)
- oluşturduğumuz `webclient.builder` i çağırarak isteği göndeririz.
- `.retrive` kodu al demek 
- `.block` devam etme sync çalıış demek
- `.bodytomono` ile gelen isteği html den integer e çevirdik.
### OpenFeign - Spring Cloud

`pom.xml` de 
`properties` de önce spring cloud versiyonu belirttik
![](ekler/Pasted%20image%2020240320210019.png)
daha sonra `dependencyManagement` eklemek gerekiyor
![](ekler/Pasted%20image%2020240320205922.png)
`dependency` ekledik
![](ekler/Pasted%20image%2020240320205842.png)


![](ekler/Pasted%20image%2020240320210147.png)

`cliend` klasörü oluşturuldu
![](ekler/Pasted%20image%2020240320210228.png)

`OrderServiceClient`

![](ekler/Pasted%20image%2020240320210501.png)
* burada  `url` kısmında direk link vermeden kullanımı aşağıda anlatılacak   `Dynamic url` & `load balancing`
- aynı bir controller gibi nereden hangi url de istek göndereceği ve parametreleri belirttik.
![](ekler/Pasted%20image%2020240320210557.png)
- daha sonrasında bunu bir değişken olarak `ServiceImpl` da kullanabiliriz.


### Netflix  Eureka -  Dinamik url oluşturulması - Service Discovery

![](ekler/Pasted%20image%2020240320211300.png)
![](ekler/Pasted%20image%2020240320211531.png)

- Netflix  Eureka Server
![](ekler/Pasted%20image%2020240320211820.png)
- microservis i tanımlaması için module ekliyoruz.
![](ekler/Pasted%20image%2020240320211857.png)
- pom.xml dosyasını seçiyoruz
![](ekler/Pasted%20image%2020240320211901.png)
-  `DiscoveryServerApplication` da yapılması gereken düzenleme
![](ekler/Pasted%20image%2020240320212016.png)
- eski versiyonlarda biraz farklı.
![](ekler/Pasted%20image%2020240320212641.png)
- `application.properties` de port belirlenir.
- Diğer konfigrasyonlar eklenir.

![](ekler/Pasted%20image%2020240320212317.png)

:8888 portunda Eureka arayüzü görünür
![](ekler/Pasted%20image%2020240320212353.png)

- Tanıtmak istediğimiz microservis e eureka client service  `dependency` si eklenir.

![](ekler/Pasted%20image%2020240320212523.png)
![](ekler/Pasted%20image%2020240320212527.png)




## Dinamik Url oluşturma

![](ekler/Pasted%20image%2020240328211736.png)
* bunu yapmanın en kolay yolu `url` vermemektir

## Load balancing 
* Bunu otomatik olarak  openfeign kütüphanesi yapmakta.
* çalışması için microservis de kaç adet instance olarak çalışacağını belirlenmesi gerekiyor. 

## Async iletişim 
![](ekler/Pasted%20image%2020240320211640.png)
- ileride daha detaylı  açıklanacak


[Spring Cloud GateWay](https://spring.io/projects/spring-cloud-gateway)

![](ekler/Pasted%20image%2020240328235558.png) 

* Tüm `microservice` lerin portlarının bilgisini alabilmesi içiin `Eureka Discovery Client` da eklememiz gerekiyor.
* Service ismini  `GatewayServer` olarak koyduk

![](ekler/Pasted%20image%2020240329001411.png)
-  uri belirtirken  microservice i hangi isimle tanıttıysak onu vermemiz gerekiyor ki  `Eureka Discovery Client` tanıyabilsin
![](ekler/Pasted%20image%2020240329001343.png)

- aynı zamanda `predicates` belirtirken  `Path=/api/customers/**` sondaki iki **  devamı değişebilen bir yapı olduğunu belirtir ve farklı isteklerde  ilgili microservice'e yönelendirme yapar 
![](ekler/Pasted%20image%2020240329001551.png)

![](ekler/Pasted%20image%2020240329001920.png)
- 8001 portunda yani `GateWay`'i kurduğum porttan tüm istekleri gönderebiliriz artık.
![](ekler/Pasted%20image%2020240329002953.png)
- burada liste içindeki bir değişkeni `docker-copose` da düzenlemek biraz zorlayıcı olacağı için  bir değişkene atayarak kullandık.
![](ekler/Pasted%20image%2020240329003200.png)

- `Docker-compose` da `localhost` değişkenlerini microservis isimleri ile değiştirmemiz gerekiyor.






# Api GateWay

- frontend tek bir noktaya istek atar api gateway de bunu ilgili microservice e bağlar 

![](ekler/Pasted%20image%2020240328234840.png)




# Configrasyon - Config Server - Config Client

![](ekler/Pasted%20image%2020240329143532.png)
![](ekler/Pasted%20image%2020240329143615.png)
![](ekler/Pasted%20image%2020240329143723.png)
![](ekler/Pasted%20image%2020240329143851.png)

* bu güncelleme ile github üzerinden config i çekiyor
![](ekler/Pasted%20image%2020240329144457.png)


- servislerde bu bilgileri almak için önce ilgili `dependency` eklemek gerekiyor 
 ![](ekler/Pasted%20image%2020240329144548.png)



# Authentication — Authorization

![](ekler/Pasted%20image%2020240403201819.png)

burada gerekli `dependencyleri` görebilirsiniz

daha sonrasında daha önce yaptığımız gibi modülü `import` etmemiz gerekmekte.


Default security ayarlarını ezmek için `configration` dosyası  yazılır

- güvenlikle farklı adımlar vardır ve biz bu adımları konfigüre edebiliyoruz

![](ekler/Pasted%20image%2020240403202958.png)
- Sen securitynin nasıl uygulanacğaını belirlemek için `config` dosyası yazılır.  
- İlk başta monolit bir yapı gibi düşünerek için core içerisinde `SecurityConfigration` dosyası oluşturduk. 

![](ekler/Pasted%20image%2020240403202917.png)
- bu dosyada güvenlik halkası bir zincir yapısında özelden genele kurallar belirterek yazılır.

![](ekler/Pasted%20image%2020240419235201.png)
- hali hazırda tanımlı olan `SecurityFilteTChain` i düzenlemek için `@Bean` anatasyonu ile yazdık
- içeriğini kendimiz oluşturmak istediğimizi belirttik. 

![](ekler/Pasted%20image%2020240419235359.png)
- alacağı parametreleri belirledikten sonra.  `try cach` ya da `throws Exception` ile kullanmamız gerekiyor.  Sorgular hata dönebileceği için
- daha sonrasında   `HttpSecurity` içerisindeki  bahsettiğimiz chain yapısını kullanarak işlemler yaparak zinciri tanımlıyoruz.
![](ekler/Pasted%20image%2020240419235632.png)

-  aynı dosya içerisinde  sadece bir configrasyon be bir `@Bean` yapısı olmasını tercih ederiz o yüzden diğer konfigrasyon için  `ApplicationConfigration`  dosyası da oluşturuyoruz. 
- burada `PasswordEncoder`  konfigrasyonlarını belirleyeceğiz. 



- `spring security` de belirli  fieldlerin olması zorunludur. 
- bu sebeple  `spring security` içerisinden gelen `UserDetai1s` i implemente etmemiz gerekiyor. 
![](ekler/Pasted%20image%2020240420000810.png)
![](ekler/Pasted%20image%2020240420000715.png)


![](ekler/Pasted%20image%2020240420000939.png)
- aynı şekide `service` de de `exstends` etmemiz gereken `UserDetai1sService` i ekliyoruz  
- amacımız bu class içindeki `loadUserByUsername` methodunu kullanmak.


# JWT 

`json web token` 
giriş yapmış kullanıcıya `encrypte` edilmiş bir `token` verilir kullanıcı bunu headera ekler ve kimin giriş yaptığı anlaşılır

![](ekler/Pasted%20image%2020240403201223.png)

![](ekler/Pasted%20image%2020240420002822.png)
- `jwt` için `jjwt` kullanacağız
![](ekler/Pasted%20image%2020240420002900.png)
- eklememiz gereken bağımlılıklar

- `AuthService` içerisinde `core` içerisinde `jwt` paketi oluşturuyoruz

- `jwt` 3 bölümden oluşur  
- `header` : hangi algoritma ile encrypte edildi 
- `payload` : özelleştirilebilir  uygulama özeli içermesi gereken bilgiler - kullanıcı ismi id si gibi gerekirse  
- `imza` :  `encryption`  geri döndürülebilir olduğu için imzasını burada kayıt ederiz.  imza dediğimiz şey encryotion un gerçekleştirildiği noktadaki anahtar. bu anahtar ile tekrar çözdüğümüz zaman değerler eşit ise o `jwt`nin benim tarafından kontrol edilip edilmediğini anlarım

- `jwtservice`  içerisindeki kodlar blue print olur. Dökümantasyondan alınır. 

![](ekler/Pasted%20image%2020240420004312.png)

![](ekler/Pasted%20image%2020240420004137.png)
- `@Value` anatasyonu ile properties dosyasından bu değerleri alırız. ve bu değerleri zaman içerisinde güncellememiz gerekir. 

![](ekler/Pasted%20image%2020240420004632.png)

![](ekler/Pasted%20image%2020240420004514.png)
- burada  kullanıcı login olduğunda kullanıcı için jwt oluşturduğumuz esnada kullanıcı için `claim` oluşturma zorunlu tutulmuş. 
- Java da default value diye bir tanım yok !!!
- Bunu method overloading yaparak  çözebiliriz. Burada `generateToken` methodu için boş bir claim hashmap i gönderen method yazdık

## JWT isteği alma 

kullanıcı jwt yi verdikten sonra kullanıcının onu nerede ttuğu değil her istekte geçerli bir token in `authorization` isimli header la gönderilmesidir.

bunu kontrol eden mekanizmaya `filter` denir 

`JwtAuthFilter`

- her istekde çalışacak bir filtre uygulamak için   extends `OncePerRequestFi1ter`  ekledik
![](ekler/Pasted%20image%2020240420011342.png)
- `doFi1terInternaLl` bu methodu override etmek zorunlu
![](ekler/Pasted%20image%2020240420011509.png)


> [!NOTE] Genel işleyişi
> Jwt nin genel işleyişi kullanıcıya belirlediğimiz anahtar ile bir token oluşturma algoritması entegre etmek. bunun için farklı kütüphaneler mevcut. 
> daha sonrasında bu token ile gelen istekde bu jwt nin doğrulunu kontrol etmek
> burada `JwtService`  token oluşturmaktan 
> `JwtAuthFilter` de gelen token in doğruluğuna bakar 

![](ekler/Pasted%20image%2020240420013112.png)

- buradaki bu satır komut satırında pin oluşturmak yerine jwt ile çalışması ve zincirde bu yapıyı kullanması gerektiğini belirtiyor.


![](ekler/Pasted%20image%2020240420012706.png)



> [!NOTE] Secure
> Her microservice in secure yapıya ihtiyacı olabilir bunun için hepsine tek tek eklenmesi gerekir
> hepsine tek tek eklemek yerine kendi paketimizi oluşturacağız
> dependency olarak eklendiğinde secure yapı implemente edilmiş olunacak.


# Authorization - Roles


![](ekler/Pasted%20image%2020240420121155.png)

- burada post u sadece role kontrolu yaparak almayı ayarlayacağız. 

![](ekler/Pasted%20image%2020240420121356.png)
- nasıl `userdetails` deki implementasyonları `spring security` için implement ettik burada da  role için `GrantedAuthority` `implemente` etmemiz gerekiyor ki kendi gereksinimlerini kullanabilelim
![](ekler/Pasted%20image%2020240420121534.png)
- role ve kullanıcı arasında  `mtm` bağlantı kurmamız gerekir. Zaten Collection olarak belirtilmiş 
![](ekler/Pasted%20image%2020240420121634.png)

- buradaki `MTM` bağlantısında 3. bir tablo oluşturmadan `spring jpa` paketini kullanarak  `entity` kullanmadan oluşturabiliriz.
![](ekler/Pasted%20image%2020240420122634.png)
- genelde Entity bağlantılarında `SetRole` olarak kullanılır
- üçüncü tablonun kaydını `@joinTable` anatosyonu içerisinde yapılır
- `name` de tablo ismi 
- `joinColumns` da içinde olduğum tablo
- `inverseJoinColumns` da bağlanacağı tablo

bu kurulumu iki entityden birisinde  yapabiliriz. Sadece `joinColumns` `inverseJoinColumns`  u ona göre configure etmek gerekir

- bu kullanımda  `MTM` tabloda ekstra bir bilginin olmaması gerekmekte 

![](ekler/Pasted%20image%2020240420124352.png)

- burada  `Collection` olarak direk `roles` entity değiilkeni dönebilir
- `Collection` `GrantedAuthority` extent ediyor role de aynı  şekilde   `GrantedAuthority` extent ediyor 



`SecurityConfigration`

![](ekler/Pasted%20image%2020240420124814.png)

> [!NOTE] Lower Case
> - roller metinsel olarak karşılaştırılır büyük harf yazımına dikkat etmemiz gerekir.



## White list url
- hangi url lere erişimi açacağımız belirleriz

![](ekler/Pasted%20image%2020240420132257.png)
- dışarıya açılmak istenen url lerin eklenmesi daha güvenlidir
![](ekler/Pasted%20image%2020240420132356.png)

- iki satır arasına özel konfigrasyonlar eklenir. 

![](ekler/Pasted%20image%2020240420133224.png)



# Kendi bağımlılığımızı oluşturmak 

![](ekler/Pasted%20image%2020240420150138.png)

intelij idea için 
![](ekler/Pasted%20image%2020240420150231.png)- ama biz cmd üzerinden yapacağız 

![](ekler/Pasted%20image%2020240420150316.png)
![](ekler/Pasted%20image%2020240420150420.png)

> [!NOTE] Neden kendi paketimiz ?
> tüm servislerim tüm `jwt` yazmaya okumaya doğrulamaya ihtiyaç duyacağı için bunu ortak bir kütüphanede yazıp diğer `microservicelere` import edersem tek bir yerden kontrol edebilirim 

- `jwt dependencyleri` 
![](ekler/Pasted%20image%2020240420151017.png)

- `JwtService`
![](ekler/Pasted%20image%2020240420151115.png)

- `Springframework` ü entegre etmemiz gerekiyor bu paket içerisine. Çünkü security kullanıyor


![](ekler/Pasted%20image%2020240420151308.png)
- security den gelen bir alan o yüzden `security` dependencysi eklenmesi gerekiyor 


pom.xml in olduğu dizinde çalıştırılması gereken komutlar 

- `maven install`
![](ekler/Pasted%20image%2020240420152024.png)
mvn repoda bu paketi oluşturdu ve artık bu paketi bağımlılık olarak ekleyebiliriz.

ve bu paketi kullanmak için `dependency` ekleyebilrim  
![](ekler/Pasted%20image%2020240420152154.png)

![](ekler/Pasted%20image%2020240420154858.png)
- bir `@Bean` olarak import etmesi için bu kullanım gerekir
- ilerleyen adımlarda best practice nasıl olur gösterilecek.



> [!NOTE] güncellemeler
> herhangi bir güncelleme yapıldığında  tekradan `mvn install` yapılması gerekir




# Secure service parçalanması
- her kural ortak olmayabilir security implementasyonunu parçalanacak


# Kafka

![](ekler/Pasted%20image%2020240424181822.png)
sync : `openfeign`  `webflux`
async : `kafka` `RubbitMq`


örneğin sipariş oluşturduğumuzda bildirim gönderen servis çalışacağı zaman `async` çalışması ya da fatura oluşturma sisteminin `async` çalışması daha mantıklıdır.

`Message Queue sistemleri` 

![](ekler/Pasted%20image%2020240424182153.png)


docker-compose.yml

![](ekler/Pasted%20image%2020240424182506.png)

`dependency`

![](ekler/Pasted%20image%2020240424182759.png)

`application.properties`

* `kafkanın` çalıştığı portunu ve `default topic` ismini belirtmemiz gerekiyor 
* Ayrıca  verdiğimiz değerler `key value` olarak iletilir  ve bu değerlerin nasıl `serialize` edileceğini yine `properties` dosyasında belirtiriz.


 `kafkada` iki adet `keyword` var 
 `producer` : mesajı yayınlayan taraf
 `consumer` : mesajı kullanan - tüketen taraf
## producer
![](ekler/Pasted%20image%2020240424183200.png)


`controller`

![](ekler/Pasted%20image%2020240424183416.png)
![](ekler/Pasted%20image%2020240424183525.png)

## consumer 


`application.properties` 

![](ekler/Pasted%20image%2020240424184206.png)
artık `producer` değil `cunsumer` olacak
artık `serialize` değil `deserializer` yapacak 

`Serialize`  bir veriyi biçime dönüştürmek  = yazıcı 
`Deserialize`  : veriyi bir biçimden geri dönüştürmek  = kuyucu

son olarak group id yi de  değiştirdik. 

`serviceAplication`

![](ekler/Pasted%20image%2020240424184432.png)
- topiğe gelen tüm mesajları yakalayacak

kafka kullanırkan `serialize` ve `deserialize` yapacağımız zaman hangi tür veri ile çalıştığımız önemlidir. basit bir örnekte `string` veri tipi için örnek verdik fakat genelde `kafka` ile `json` formatında karmaşık veriler kullanılır. Bunun için de kullanmış olduğumuz `string.serializerler` uygun `serializer` ayarları ile dökümantasyon'a göre değiştirilmelidir




