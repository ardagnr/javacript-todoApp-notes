# OAuth2

OAuth 2.0, kullanıcıların başka bir uygulama üzerinden kendi verilerine güvenli bir şekilde erişim yetkisi vermesini sağlayan bir yetkilendirme protokolüdür. Örneğin, bir web sitesi veya mobil uygulama, kullanıcıya ait bir kaynağa erişmek için OAuth 2.0 kullanabilir.

OAuth 2.0, kullanıcı adı veya şifre paylaşmadan, bir uygulamanın kullanıcının diğer hizmetlerdeki kaynaklarına erişmesine olanak tanır.

### resource owner 
Kaynağın sahibidir. Genellikle bir kullanıcıyı ifade eder. Örneğin, bir kullanıcının Google Drive'daki belgeleri.

### resource server
Korunan kaynakları barındırır. Kaynağa yapılan istekleri kabul eder ve doğru token ile gelen isteklere yanıt verir. Örneğin, Google Drive API, bir kaynak sunucusudur.

### client
Kullanıcı adına korunan bir kaynağa erişmek isteyen uygulamadır. Örneğin, bir web sitesi veya mobil uygulama. İstemciler, kaynak sahibinin izniyle API'lere erişir.

### authorization server
Access token’ları oluşturur ve clientin kullanımına sunar. Authorization server, istemciden gelen yetkilendirme kodlarını doğrular ve karşılığında bir access token verir.

## OAuth2 flow

### Authorization Code Grant
1. **Client, Authorization Servera** istek atar ve bu sayede **Resource Ownerdan** yetki ister.
2. **Resource Owner** yetki verir.(Yetki vermezse işlem sonlanır)
3. Bunun sonucunda **Authorization Server** **Cliente** bir Authorization Code gönderir.
4. **Cliente** bu kodu tekrar **Authorization Servera** gönderir.
5. Eğer geçerli bir kod gönderilmişse **Authorization Server** bir token oluşturur ve **Cliente** gönderir.
6. **Clientte** bu token ile beraber **Resource Server** ile istediği kaynağa erişebilir.

**Eğer yetki kaldırılırsa token geçerliliğini yitirir ve erişim kısıtlanır.**


### Implicit Grant

### Resource Owner Password Credentials Grant

Kullanıcı adı ve şifre ile erişim sağlanır. Ancak kullanıcı adı ve şifre istemci tarafından alınır, bu da güvenlik sorunlarına yol açabilir. Modern uygulamalarda artık pek kullanılmaz.

### Client Credential Grant

Resource owner izin vermeden uygulamanın korumalı kaynağa erişebilmesine denir.

## Client Oluşturma

**https://console.cloud.google.com/**

- APIs & Services altında Credentials kısmına gelerek burada **Create Credentials** ile yeni bir OAuth clienti oluşturuyoruz.

- Sonrasında bir Client ID, Client Secret,Project_id,Auth_url,Token_url,Auth_provider değerleriyle karşılaşacağız.

- Artık bunu test edebilmek için postman veya insomnia gibi uygulamaları kullanabiliriz.
- Yeni bir request oluşturarak **Auth** kısmına geliriz.
-Grand Typeımız Authorization Code.
- Authorization Url ise kullanıcının yetkilendirilmesi için yönlendirileceği url (auth_url).
- Access Token Url ,Token alabilmek için authorization serverın hangi urlsine istek atacağımdır(token_url).
- Redirect Url, kullanıcı yetki verdikten sonra authorization serverın nereye yönlendireceğidir.
- Scope ,örneğin google hesabı ile giriş yapmış bir kullanıcının hangi bilgisine ulaşmak istediğimizi belirttiğimiz yerdir.

Sonuç olarak bize bir token döner ve bu token sayesinde istediğimiz bilgilere ulaşabiliriz.