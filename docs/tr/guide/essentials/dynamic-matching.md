# Dinamik Rota Eşleştirme

Genellikle, verilen desene sahip rotaları aynı component'e eşlememiz gerekir. Örneğin, tüm kullanıcılar için aynı düzene sahip ancak farklı bir kullanıcı ID'si ile işlenmesi gereken bir `User` component'imiz olabilir. `vue-router` içindeki path'te dinamik segmentler kullanarak bunu yapabiliriz:

``` js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // dinamik segmentler iki nokta üst üste ile başlar
    { path: '/user/:id', component: User }
  ]
})
```

Artık `/user/foo` ve `/user/bar` gibi URL'ler aynı rota ile eşlenecektir.

Dinamik segment iki nokta üst üste işaretiyle gösterilir `:`. Bir rota eşleştirildiğinde, her component'in dinamik segmentinin parametre değeri `this.$route.params` ifadesi ile mevcut component içinde elde edilebilir. Bu nedenle, `User` şablonunu aşağıdaki gibi güncelleştirerek geçerli kullanıcı kimliğini görüntüleyebiliriz:

``` js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

[Buradan](https://jsfiddle.net/yyx990803/4xfa2f19/) çevrimiçi örneklere göz atabilirsiniz.

Aynı rota üzerinde birden fazla dinamik segmentiniz olabilir ve bunlar `$route.params` adresindeki ilgili alanlara eşlenirler. Örneğin:

| desen(pattern)                | eşlesen yol         | $route.params                          |
| ----------------------------- | ------------------- | -------------------------------------- |
| /user/:username               | /user/evan          | `{ username: 'evan' }`                 |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |

`$route.params`'a ek olarak, `$route` nesnesi, `$route.query` (URL'de bir sorgu varsa), `$route.hash` vb. gibi diğer yararlı bilgileri de gösterir. Bunlar hakkında daha fazla bilgi için [API Referans](../../api/#the-route-object) sayfasına bakabilirsiniz.

## Rota parametrelerindeki değişikliklere tepki verme
Parametreleri olan rotaları kullanırken dikkat edilmesi gereken bir nokta, kullanıcı `/user/foo`'dan `/user/bar`'a gittiğinde **aynı component örneğinin(instance) yeniden kullanılacağıdır**. Her iki rota da aynı component'i oluşturduğundan, bu yaklaşım bir component örneğini yok edip yeniden oluşturmaktan daha verimlidir. **Bununla birlikte, bu aynı zamanda component'in life cycles(yaşam döngüsü) hook'larının çağrılmayacağı anlamına gelir.**.

Mevcut component'teki parametre değişikliklerine tepki vermek için `$route` nesnesini izlemeniz yeterlidir:

``` js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // parametre değiştiğinde çalışacak kodlar..
    }
  }
}
```

Ya da 2.2'de tanıtılan `beforeRouteUpdate` [navigasyon korumasını](../advanced/navigation-guards.html) kullanın:

``` js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // parametre değiştiğinde çalışacak kodlar..
    // next()'i çağırmayı sakın unutma
  }
}
```

## Tüm rotaları veya 404 Not found rotları yakalama

Düzenli parametreler yalnızca `/` ile ayrılmış URL parçaları arasındaki karakterlerle eşleşir. Eğer **herhangi bir şey** ile eşleştirmek istiyorsak yıldız işareti(`*`) kullanırız:

```js
{
  // her şeyle eşleşecektir
  path: '*'
}
{
  // `/user-` ile başlayan herşeyle eşleşecektir
  path: '/user-*'
}
```

_Yıldız işaretli_ rotaları kullanırken, yönlendirmelerin doğru sırada olduğundan emin olun, bu da _yıldız işaretli_ rotaların en son yerleştirilmesi gerektiği anlamına gelir.
`{ path: '*' }` rotası genellikle kullanıcı tarafındaki 404 sayfasında kullanılır. Eğer _History modu_ kullanıyorsanız [sunucunuzu doğru şekilde yapılandırdığınızdan](./history-mode.md) emin olun.

_Yıldız işareti_ kullandığınızda, `$route.params`'a otomatik olarak `pathMatch` adlı bir parametre eklenir. _Yıldız işareti_ ile eşlenen url'lerin geri kalanını içerir:

```js
// { path: '/user-*' } rotası verildi
this.$router.push('/user-admin')
this.$route.params.pathMatch // 'admin'

// { path: '*' } rotası verildi
this.$router.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'
```

## Gelişmiş Eşleştirme Desenleri

`vue-router` yol eşleştirme motoru olarak [regexp-yolu](https://github.com/pillarjs/path-to-regexp) kullanır, yani isteğe bağlı dinamik segmentler, sıfır veya daha fazla / bir veya daha fazla gereksinim ve özel regex ifade modelleri gibi birçok gelişmiş eşleme desenini destekler.Gelişmiş desenler için [dokümana](https://github.com/pillarjs/path-to-regexp#parameters) bakabilirsiniz.`vue-router`ın kullanıldığı [bu örneğe](https://github.com/vuejs/vue-router/blob/dev/examples/route-matching/app.js) bir göz atabilirisiniz .

## Öncelik Eşleştirme

Bazen aynı URL birden çok rotayla eşlenebilir. Bu gibi durumlarda, öncelik yolların tanımı sırasına göre belirlenir: bir rota ne kadar erken tanımlanırsa, o kadar yüksek öncelik alır.
