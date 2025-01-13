# JavaScript Temelleri
## Veri Tipleri
- number,string,Boolean (Primitive) bu veri tipleri kendi başlarına var olabilirler.
- array,object (Primitive değil) başka veri tiplerine bağlılardır.

```javaScript
const u={};
console.log(typeof u);
```
u'nun typeını object olarak görürüz.

### Metot ve Fonksiyon Farkı 
``` javaScript

class User{
    constructor(name){
        this.name=name;
    }
    sayHi(){    //Metot
        console.log(this.name);
    }
}
function sayHi(self){ //Fonksiyon
    console.log(self.name);
}

const a = new ("arda");
a.sayHi();

sayHi(a);
```
Metotlar parametrelerini örtülü bir şekilde alabilir fakat fonksiyonlar parametrelerini açık bir şekilde almalıdır.

Metotlar her zaman biz bir parametre vermeden parametre olarak this parametresini alır.

Fonksiyon olarak tanımladığımız sayHi ise this parametresini alamıyor.

Static metotlar fonksiyonlara birebir denktir.

Fakat sayHi.bind(a)();  ifadesini eklersek artık metot olarak tanımladığımız ifade bir fonksiyona dönüşüyor.( bind(a) ifadesi ile artık this paramatresinin a'nın kendisi olduğunu belirtmiş oluyoruz ) 

Fonskiyonları da bir metot olarak kullanabiliriz.
```javascript
function createUser(name){    
    return{ 
        name,
        sayHi: () => {
            return `Hi, my name is ${name}`
        },
    };
}

const u = createUser("Arda");

console.log(u.sayHi());

```
Bu ifadede sayHi fonksiyonuna erişebiliyoruz fakat her createUser fonksiyonu çağırıldığında sayHi fonksiyonu tekrar tekrar oluşuyor bu da hafızada fazla yer kaplanmasına sebep oluyor.Bu sebeple ifadeyi aşağıdaki gibi değiştirebiliriz.Aşağıdaki kodda __proto__ sayesinde userMethodsa erişebiliyoruz. Öncelikle bir User oluşturuluyor daha sonra bu User'ın sayHi adında geri dönen bir fonskiyonu var mı buna bakılır. Olmadığı için de __proto__ ifadesi bir objeyi işaret ediyor ve o objenin içinde de sayHi fonksiyonumuz bulunuyor bu sayede erişmiş oluyoruz.


```javascript
//Bu ifade 
const userMethods={
    sayHi: function(){
        return `Hi, my name is ${this.name}`;
    }
};

function createUser(name){
    const user={
        name
    };

    user.__proto__ = userMethods;
    
    return user;
}

const u = createUser("Arda");

console.log(u.sayHi());
```

Her fonksiyon aslında bir obje geriye döndürür . Classlar da bu işi yapar o halde biz Class ifadelerini birer fonksiyon halinde yazabiliriz.

```javaScript
class User{
    constructor(name){
        this.name=name;
    }
}

function User(name){ 
    this.name=name;
    return this; //burdaki işlemler aslında class sınıfının içindeki constructor metotunun işlevini yerine getirdi.
}

const u = new User("Arda"); // new ifadesi ile fonskiyonların içinde kullanamadığımız this parametresi kullanılabilir hale geldi.

//new ifadesi aslında aşağıdaki işlemleri yapıyor.
const u ={}; // boş obje oluşturulur.
User.call(u,"Arda"); // ilk parametresini açıkça vermek istersen call ifadesini de kullanabiliriz.
u.__proto__= User.prototype;

//artık yazdığımız fonksiyon bir class ifadesine denk geliyor.
```
## Package Manager

Hazır olarak kütüphaneleri indirmemize yarar.

npm,yarn,npx
### npm
- Bir proje oluşturalım **npm init -y**
- Express kütüphanesini kuralım **npm install express**
- Artık package.json sayfamız var ve içinde dependencies kısmı var. Burası bizim projemizde kurulu olan tüm paketleri sürümleriyle beraber yazar. Sürümler bir aralık şeklinde belirtilir. Bu sayfa sayesinde tek bir ifade ile tüm kütüphaneleri yükleyebiliriz. Bu dosya Git reposunda bulunmalı.
- **npm install** ile package.json içinde tüm kütüphaneleri yükleyebiliriz.
- **npm install "kütüphane adı"** bu şekilde sadece adını girdiğimiz kütüphaneyi indiririz.

**Node.js package.json dosyasına bakmaz sadece node_modules klasörümüzün içine bakar.**
node_modules klasörü Git reposunda bulunmaz.
```javaScript
const express = require("express");//bu şekilde kütüphaneyi projemize ekliyebiliriz
```
### nodemon
- Bir dosyayı değiştirdiğimiz zaman o dosyanın tekrar çalıştırılmasını sağlar.
- **npm install -D nodemon**

```javaScript
const express = require("express");//express kütüphanemizi projeye ekledik
const app = express();

app.get("/",(req,res)=>{
    return res.send("Hello World");
});

app.listen(3000);
```
## Functional Programming

### Side Effect:
console.log("Hello world") => ekrana bir yazı basar ve kaç defa çalıştırıldığına bağlı olarak ekranımız değişir.
### Pure Function:
Side effect olmayacak ve Same Input => Same Output olacak.

```javaScript
function myFn(x,y){//Bu fonksiyon Pure bir fonksiyondur.
    return x+y;
}
function myFn(x,y){
    console.log("Hello World");//Bu fonksiyon Pure bir fonksiyon değildir.
    return x+y;
}
```
### Filter:
Filter bir true-false değer döndürür ve true olanları filtreler.

```javaScript
const arr=[1,2,3,4,5,6,7,8,9,10];

const result=arr.filter(x => x%2 == 0)

console.log(result);//[2,4,6,8,10]
```
### Map:
Map içerisinde aldığı işlemi gerçekleştirir ve o değeri döndürür.
```javaScript
const arr=[1,2,3,4,5,6,7,8,9,10];

console.log(arr.map(x => x*2 ));//[2,4,6,8,10,12,14,16,18,20]
```
### Every ve Some:
Listenin içindeki her eleman ve bazı elemanlar istediğimiz koşulu sağlıyor mu onu kontrol etmemize yarar.
```javaScript
const arr=[1,2,3,4,5,6,7,8,9,10];

console.log(arr.every(x => x<5));//false

const arr=[1,2,3,4,5,6,7,8,9,10];

console.log(arr.some  (x => x<5));//true
```

### Reduce
```javaScript
const arr=[1,2,3]
//Başlangıç değeri:0
//fn(0,1) => x
//fn(x,2) => y
//fn(y,3) => z

console.log(arr.reduce((a,x) => a+x,0));//6

```
### Find ve FindIndex
```javaScript
const numbers=[1,2,3,4,5,6,7,8,9,10]

const found = numbers.find((element) => element>5);

console.log(found);//6

//findIndex() ise o elemanın indeksini buluyor.

```
**Bir fonskiyon başka bir fonksiyon geri döndürebilir**
```javaScript
function add(x){
    return function(y){
        return x+y;
    }
}
console.log(add(1)(2));// 3
```

