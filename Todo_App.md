# Command Line
```bash
# Proje için bir klasör oluşturur
mkdir tdr

# Klasörün içine girilir
cd tdr

# Git reposu başlatır
git init

# .gitignore ile git reposuna yollamak istemediğimiz dosyaları belirleriz.
curl https://www.toptal.com/developers/gitignore/api/linux,macos,windows,node > .gitignore

# package.json dosyası eklenir
npm init -y

# source klasörü ve source altında index.ts dosyası oluşturulur
mkdir src
touch src/index.ts

# .github klasörü oluşturulur
mkdir .github

# vs code ayarlarını tutmak için bir dosya oluşturulur
mkdir .vscode

# prettier kurulumu yapılır
npm i -D prettier
touch .prettierrc

# eslint kurulumu yapılır
npm i -D eslint
touch .eslintrc

# TypeScript ve tsx kurulumu yapılır
npm i -D typescript tsx @types/node
npx tsc --init

# Readme.md dosyası oluştulur
touch README.md
```
package.json içine bunlar eklenir
```json
  "main": "src/index.ts",
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "start": "tsx src/index.ts",
    "dev": "tsx --watch src/index.ts"
  },
```
.vscode klasörünün altında settings.json dosyası oluşturulup bunlar eklenir

```json
{
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```
Aşağıdaki bizim bir index.ts dosyamız

```Javascript
import process from 'process';
import fs from 'fs';
import crypto from 'crypto';

// Todo Tanımlaması Yapılır
interface Todo {
  id: string;
  title: string;
  description: string;
  completed: boolean;
}

async function main() {

  //komut satırı argümanlarını alıyoruz
  const [program, script, subcommand, ...args] = process.argv;

  // eğer dosyamız yoksa oluşturuyoruz
  try {
    await fs.promises.stat('todos.json');
  } catch {
    await fs.promises.writeFile('todos.json', '[]');
  }
  switch (subcommand) {
    case 'add': {
      const [title, description] = args;
      //Dosyası önce okuyoruz daha sonra da parse ediyoruz.
      const fileConent = await fs.promises.readFile('todos.json', 'utf-8');
      const todoList = JSON.parse(fileConent) as Todo[];

      // Değeri kontrol ediyoruz.
      if (!title || typeof title !== 'string' || title.length < 2 || title.length > 255) {
        console.log('Title is required and its length must be between 2 and 255 characters');
        process.exit(1);
      }

      if (description && (typeof description !== 'string' || description.length > 4096)) {
        console.log('Description length must be less than 4096 characters');
        process.exit(1);
      }

      // yeni bir todo oluşturuyoruz
      const id = crypto.randomBytes(4).toString('hex');
      const newTodo: Todo = {
        id: id,
        title,
        description,
        completed: false,
      };

      // todoyu listeye ekliyoruz
      todoList.push(newTodo);

      // dosyayı güncelleyip yazıyoruz.
      const updatedFileContent = JSON.stringify(todoList, null, 2);
      await fs.promises.writeFile('todos.json', updatedFileContent);

      console.log('New todo added');
      break;
    }

      //... Diğer İşlemler yapılır
}

main();
```
## Async-Await Nedir?

Async-Await, JavaScript'te asenkron işlemleri yönetmek ve kodu daha okunabilir hale getirmek için kullanılan bir yapıdır. Promise tabanlı bir asenkron programlama modeline daha kolay bir alternatif sunar. Async-Await, özellikle uzun süren işlemleri (örneğin, veri tabanına erişim, API çağrıları gibi) senkronize bir şekilde yazıyormuş gibi kodlamamızı sağlar.

## Kod düzenlenmesi 

Tüm işlemleri tek bir dosyada yapmak okunarlılık açısından ve karmaşık programlar açısından kötü bir durumdur bu nedenle service katmanına ihtiyaç duyarız.

```javascript
//index.ts
import process from 'process';
import * as todoService from './TodoService';

async function main() {
  // Get command line arguments
  const [program, script, subcommand, ...args] = process.argv;

  switch (subcommand) {
    case 'add': {
      const [title, description] = args;
      await todoService.addTodo(title, description);
    }
//...Diğer işlemler
  }
}

main();

```
```javascript
//TodoService.ts
import fs from 'fs';
import crypto from 'crypto';

export interface Todo {
  id: string;
  title: string;
  description: string;
  completed: boolean;
}

async function readTodos(): Promise<Todo[]> {
  try {
    await fs.promises.stat('todos.json');
  } catch {
    await fs.promises.writeFile('todos.json', '[]');
  }

  const fileConent = await fs.promises.readFile('todos.json', 'utf-8');
  const todoList = JSON.parse(fileConent) as Todo[];

  return todoList;
}
async function writeTodos(todoList: Todo[]): Promise<void> {
  const updatedFileContent = JSON.stringify(todoList, null, 2);
  await fs.promises.writeFile('todos.json', updatedFileContent);
}

export async function addTodo(title: string, description: string): Promise<void> {
  const todoList = await readTodos();

  if (!title || typeof title !== 'string' || title.length < 2 || title.length > 255) {
    console.log('Title is required and its length must be between 2 and 255 characters');
    process.exit(1);
  }

  if (description && (typeof description !== 'string' || description.length > 4096)) {
    console.log('Description length must be less than 4096 characters');
    process.exit(1);
  }

  const id = crypto.randomBytes(4).toString('hex');
  const newTodo: Todo = {
    id: id,
    title,
    description,
    completed: false,
  };

  todoList.push(newTodo);

  writeTodos(todoList);

  console.log('New todo added');
}
//Diğer işlemler yapılır

```
**Bu sayede kodlarımız daha düzenli bir şekilde olmaya başladı**

**Şimdiye kadar bir database kullanmadığımız için database katmanı yoktu eğer kullanmak istersek bir database katmanı da eklemeliyiz.**

**Presentation Layer:** Tüm kullanıcı etkileşimlerini yönetir. (Komut satırı argümanlarını ayrıştırma ve sonuçları yazdırma)

**Service Layer:**  Bu katmanda ise işlemlerin nasıl yapılacağıyla ilgilenilir.Uygulamanın iş mantığıdır.

**Data Access Layer:** Bu katman veri tabanındaki işlemlerin nasıl yapılacağıyla ilgilenmez sadece gereken işlemleri yapar.(add,list,...)

```JavaScript
//TodoRepository.ts
import fs from 'fs';

export interface Todo {
  id: string;
  title: string;
  description: string;
  completed: boolean;
}

class TodoRepository {
  fileName: string = 'todos.json';

  constructor() {}

  public async readTodos(): Promise<Todo[]> {
    try {
      await fs.promises.stat('todos.json');
    } catch {
      await fs.promises.writeFile('todos.json', '[]');
    }

    const fileConent = await fs.promises.readFile('todos.json', 'utf-8');
    const todoList = JSON.parse(fileConent) as Todo[];

    return todoList;
  }

  public async writeTodos(todoList: Todo[]): Promise<void> {
    const updatedFileContent = JSON.stringify(todoList, null, 2);
    await fs.promises.writeFile(this.fileName, updatedFileContent);
  }
}

export default TodoRepository;

```
```Javascript
//TodoService.ts
import TodoRepository, { Todo } from './TodoRepository';
import crypto from 'crypto';

class TodoService {
  repository: TodoRepository;

  constructor() {
    this.repository = new TodoRepository();
  }

  public async addTodo(title: string, description: string): Promise<void> {
    const todoList = await this.repository.readTodos();

    if (!title || typeof title !== 'string' || title.length < 2 || title.length > 255) throw new Error('Title is required and its length must be between 2 and 255 characters');
    if (description && (typeof description !== 'string' || description.length > 4096)) throw new Error('Description length must be less than 4096 characters');

    const id = crypto.randomBytes(4).toString('hex');
    const newTodo: Todo = {
      id: id,
      title,
      description,
      completed: false,
    };

    todoList.push(newTodo);

    await this.repository.writeTodos(todoList);
  }
}
 //...Diğer işlemler

export default TodoService;
```
```Javascript
//index.ts
import process from 'process';
import TodoService from './TodoService';

const todoService = new TodoService();

async function main() {

  const [program, script, subcommand, ...args] = process.argv;

  switch (subcommand) {
    case 'add': {
      const [title, description] = args;
      await todoService.addTodo(title, description);
      console.log('New todo added');
    }
  //...Diğer işlemler
  }
}

main();

```
# Server Side

Bilgisayarların haberleşmesi için çeşitli yollar bulunmaktadır.

**point-to-point:** Bilgisayarların birebir haberleşmeleridir.

**bus:** Bilgisayarların ortak bir hat üzerinden haberleşmeleridir.Bir bilgisayarın gönderdiği bir veriyi tüm bilgisayarlar alabilir.

**ring:** Bilgisayarların sırayla birbirleri üzerinden iletişime geçmeleridir.

**star:** Bilgisayarlar ana bir bilgisayar aracılığıyla iletişime geçerler.

ve daha fazlası...

### OSI katmanları

Bu model, ağ iletişimini 7 farklı katmana ayırarak, her bir katmanın belirli bir görevi yerine getirmesini sağlar. Böylece, farklı donanım ve yazılım üreticilerinin sistemleri birbiriyle uyumlu çalışabilir.

**Fiziksel Katman (Physical Layer):** Donanım düzeyinde çalışır.

**Veri Bağlantı Katmanı (Data Link Layer):** Verilerin fiziksel bağlantı üzerinden hatasız bir şekilde iletilmesini sağlar.

**Ağ Katmanı (Network Layer):** Verilerin bir ağdan diğerine yönlendirilmesini ve iletilmesini sağlar.

**Taşıma Katmanı (Transport Layer):** Verilerin uçtan uca güvenilir bir şekilde iletilmesini sağlar.TCP ile haberleşiyorsak verileri gönderdiğimizde karşı tarafa ulaştığından emin oluruz. Fakat UDP ile haberleşiyorsak veriler kaybolabilir.

**Oturum Katmanı (Session Layer):** İki uygulama arasında oturum (bağlantı) başlatır, yönetir ve sonlandırır.

**Sunum Katmanı (Presentation Layer):** Verilerin uygulama katmanına uygun bir formata dönüştürülmesini sağlar.

**Uygulama Katmanı (Application Layer):** Kullanıcıların doğrudan etkileşimde bulunduğu katmandır.

```JavaScript
//Bir web sunucusu oluştururuz.İstek atıldığında ona bir html kodu geri döndürürüz.
import express from "express";

const app = express();

app.get('/', (req, res) => {
  res.send('<h1>Hello World</h1>');
});

app.listen(3000, () => {
  console.log('Server is listening on port 3000');
});

```

### Handlebars
Web uygulamalarında HTML yapısını dinamik bir şekilde oluşturmak için kullanılır. Handlebar'lar, değişkenleri, döngüleri ve koşulları kullanarak statik HTML dosyalarını dinamik hale getirmek için kullanılır.
```html
<h1>{%title%}</h1>
<ul>
  {%#each tasks%}
  <li>{%this%}</li>
  {%/each%}
</ul>

```