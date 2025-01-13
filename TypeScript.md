# TypeScript
TypeScript temel olarak bize tip güvenliği sağlar.

**npm install -D tsx** ile projemize ekliyoruz.

**npx tsx --watch "dosya_adı"** ile çalıştırabiliriz.

Her JavaScript kodu bir TypeScript kodudur.Tam tersi geçerli değildir.

**Primitive Types(number,string,boolean,null,undefined)**
```typeScript
function f(x:number){//number yerine diğer typeları da koyabiliriz bu gelecek olan değerin türünü belirler.
    console.log(x)
}
f(2);
```
**Array**
```typeScript
function f(x:(number | string)[]){//arrayin içinde number veya string olabilir.
    console.log(x)
}
f(["a",5]);

function f(x:string[]){//arrayin sadece string olabilir.
    console.log(x)
}
f(["a","b"]);
```

Typeları dışarda kendimiz de oluşturabiliriz.
```typeScript
type Vector=[number,number]

function f(x:Vector){
    const lengt :number = Math.sqrt(x[0]**2 + x[1]**2);
    console.log("Length of the vector:",length);//Length of the vector:2.6457513110645907
}
f([3,4]);

```
**Object**
```typeScript
type Vector={
    x:number,
    y:number
};

function f(v:Vector){
    const lengt :number = Math.sqrt(v.x**2 + v.y**2);
    console.log("Length of the vector:",length);//Length of the vector:2.6457513110645907
}
f({x:3,y:4});

```
```typeScript
type Person={
    name:string,
    age:number,
    gender:"Male"|"Female"
};

const Arda:Person={
    name: "Arda",
    age: 21,
    gender:"Male"
}

```
**Enum**

```typeScript
enum Gender{
    Male = "M",
    Female="F"
}

type Person={
    name:string,
    age:number,
    gender:Gender
};

const Arda:Person={
    name: "Arda",
    age: 21,
    gender:Gender.Male
}

```
**Void**

Bir fonksiyon hiçbir şey döndürmüyorsa o fonksiyonun typeına void diyebiliriz.

**Any**

Her türlü typeı kabul eder.

**Function Type ve Generics**

```JavaScript
function map(arr:number[], fn:(x:number)=> number){
    const result: number [] = [];

    for(const x of arr){
        result.push(fn(x));
    }
    return result;
}
const arr=[1,2,3,4,5];
console.log(map(arr,(x:number) => x*2));//2,4,6,8,10

//Her bir tip için ayrı ayrı yazmamak için(mapNumber,mapString) Generics kullanıyoruz.
//Fonksiyonların bir tip aldığını varsayabiliriz.

function map<T>(arr:T[], fn:(x:T)=> T){
    const result: T[] = [];

    for(const x of arr){
        result.push(fn(x));
    }
    return result;
}
const arr=["a","b","c"];
console.log(map(arr,(x:String) => "-"+x));//['-a','-b','-c']
//Bu sayede biz hangi tipte bir array gönderirsek ona göre bir işlem yapılmış olunuyor.

//Bu versiyon gönderdiğimiz tiple çıkan tip farklı ise kullanılır. 
function map<T,U>(arr:T[], fn:(x:T)=> U) :U [] {
    const result: T[] = [];

    for(const x of arr){
        result.push(fn(x));
    }
    return result;
}

const arr=[1,2,3];
console.log(map(arr,(x:Number) =>{
    switch(x){
        case 1: return "one";
        case 2: return "two";
        case 3: return "three";
        default: return "unknown";
    }
}));
```

## Class
```typeScript

var count =0;//State!!!!!!

function counter():number{
    return ++count;
}

var count2 =0;

function counter2():number{
    return ++count2;
}
console.log(counter());//1
console.log(counter2());//1
console.log(counter());//2

```
Bu şekilde yönetemeyiz bu süreci

**JavaScriptte number,string,boolean değişkenler call by value olarak gönderilir.**

**Object ve arrayler call by reference olarak gönderilir.**
```typeScript
//Call by value
function inc(state:number):number{                  
    return ++state;
}

var count1 =0;//State!!!!!!

var count2 =0;//State!!!!!!


console.log(inc(count1));//1
console.log(inc(count1));//1
console.log(inc(count2));//1
console.log(inc(count2));//1
```
```typeScript
//Call by reference
type Sayacstate ={
    count: number
}

function inc(state:Sayacstate):number{                  
    return ++state.count;
}

var count1 :Sayacstate ={count : 0};//State!!!!!!

var count2 :Sayacstate ={count : 0};//State!!!!!!


console.log(inc(count1));//1
console.log(inc(count1));//2
console.log(inc(count2));//1
console.log(inc(count2));//2

```
Bunu bir class yapısına dönüştürecek olursak:
```JavaScript
class Sayac{
    count:number;
    constructor(count:number){
        this.count=count;
    }
    inc(): number{
        return ++this.count;
    }
}

var count1:Sayac =new Sayac(0);
var count2:Sayac =new Sayac(0);

console.log(count1.inc());//1
console.log(count1.inc());//2
console.log(count2.inc());//1
console.log(count1.inc());//3
```

## Interface
Interface, TypeScript'te bir nesnenin veya sınıfın sahip olması gereken özelliklerin ve yöntemlerin yapısını tanımlayan bir sözleşmedir.Örneğin aşağıdaki örnekte SayacA ve SayacB adında iki tane class var ve ortak olarak kullandıkları belli metotlar var(inc(),reset()). Bunları her iki classta da tanımlamaktansa bir interface ile tanımlayabiliriz.

**Eğer bir interface çağırılıyorsa içinde bulunan tüm metotlar kullanılmak zorundadır ve bu classlardaki metotlar farklı işlevleri yapabilirler**
```JavaScript
class SayacA{
    count:number;
    constructor(count:number){
        this.count=count;
    }
    inc(): number{
        return ++this.count;
    }
    reset():void{
        this.count=0;
    }
}

class SayacB{ //fazladan dec() metotuna sahip
    count:number;
    constructor(count:number){
        this.count=count;
    }
    inc(): number{
        return ++this.count;
    }
    reset():void{
        this.count=0;
    }
    
    dec():number{
        return --this.count;
    }
}

function User(sayac:SayacA|SayacB){
    for(let i=0; i<100; i++){
        sayac.inc();
    }
    console.log(sayac.count);
}
User(new SayacA(0));
USer(new SayacB(0));

```
```JavaScript
interface Sayac{
    inc(): number;
    reset(): void;
    getCount(): number;
}

class SayacA implaments Sayac{
    count:number;
    constructor(count:number){
        this.count=count;
    }
    inc(): number{
        return ++this.count;
    }
    reset():void{
        this.count=0;
    }
    getCount():number{
        return this.count;
    }
}

class SayacB implaments Sayac{ 
    count:string;
    constructor(count:number){
        this.count=String(count);
    }
    inc(): number{
        const x = Number(this.count);
        const incremented = x + 1;
        this.count= incremented.ToString();
        return incremented;
    }
    reset():void{
        this.count="0";
    }
    
    // dec():number{           
    //     return --this.count;
    // }
    getCount():number{
        return Number(this.count);
    }
}

function User(sayac:Sayac){
    sayac.reset();
    for(let i=0; i<100; i++){
        sayac.inc();
    }
    console.log(sayac.getCount());
}
User(new SayacA(0));
USer(new SayacB(0));

```
### TypeScript Compailer
**npm install -D typescript** ile typescripti yükledikten sonra **npx tsc ".ts dosyamızın adı"** ile derleyebiliriz.Derlendiğinde dosyamızı javascript dosyası olarak tekrar oluşturur.

**tsc --init** komutu ise bir **tsconfig.json** dosyası oluşturur. Bu dosyanın içerisinde typescript ve javascript compailerının alacağı parametreler vardır ve ayarlarını yapmamıza yarar.