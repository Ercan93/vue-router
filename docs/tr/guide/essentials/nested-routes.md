# İç içe rotalar

Gerçek bir uygulamanın kullanıcı arabirimi genellikle hiyerarşik olarak iç içe olan component'lerin birden çok düzeyinden oluşur. Benzer şekilde, URL'deki her segmentinin dinamik rotaları, aşağıdakiler gibi bir yapıdaki iç içe gelişmiş katman component'lerine karşılık gelir:

```
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

`vue-router` ile, bu ilişkiyi iç içe rota yapılandırmaları kullanarak ifade etmek çok basittir.

Önceki bölümde oluşturduğumuz uygulamaya bir bakalım:

``` html
<div id="app">
  <router-view></router-view>
</div>
```

``` js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

Burada `<router-view>`, üst düzey rotaya karşılık gelen component'in çıkış noktasını temsil etmektedir.Bu, üst düzey rotaya uyan component'leri işlemektedir(render eder). Benzer şekilde render edilmiş bir component'te kendi içinde `<router-view>` içerebilir. Örneğin `User` component şablonunu biraz değiştirelim:

``` js
const User = {
  template: `
    <div class="user">
      <h2>Kullanıcı {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```

Component'leri bu iç içe noktada görüntülemek için, `VueRouter` oluşturucu yapısındaki `children` seçeneğini kullanmamız gerekir :

``` js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // rota /user/:id/profile ile eşleştiği zaman
          // UserProfile component'i, User'ın içindeki <router-view>'de işlenecektir.
          path: 'profile',
          component: UserProfile
        },
        {
          // rota /user/:id/posts ile eşleştiği zaman
          // UserPosts component'i, User'ın içindeki <router-view>'de işlenecektir.
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

**`/` ile başlayan iç içe giden yolların, kök yolları olarak kabul edildiğini unutmayın. Bu, iç içe geçmiş bir URL kullanmak zorunda kalmadan iç içe component kullanmanıza olanak tanır.**

Gördüğünüz gibi `children` seçeneği, rotaların kendisi gibi `routes` yapılandırma nesnelerinin bir başka dizisidir. Bu nedenle istediğiniz kadar iç içe görüntüleme elde edebilirsiniz.

Bu noktada, yukarıdaki yapılandırmada `/user/foo` adresini ziyaret ettiğinizde, hiçbir alt rota eşleşmediği için `User`'ın çıkışında(içerisindeki router-view'da) hiçbir şey görüntülenmez. Orada bir şey görüntülemek istiyorsanız, boş bir alt kök yolu ayarlayabilirsiniz:

``` js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // rota /user/:id ile eşleştiğinde
        // UserHome component'i, User'ın  içindeki <router-view>'da görüntülenecektir.
        { path: '', component: UserHome },

        // ...diğer alt rotalar
      ]
    }
  ]
})
```

Örneğin çalışan bir demosunu [burada](https://jsfiddle.net/yyx990803/L7hscd8h/) bulabilirsiniz.
