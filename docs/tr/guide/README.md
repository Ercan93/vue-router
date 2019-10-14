# Başlarken

<Bit/>

::: tip Not
Bu rehberdeki örnek kodlar [ES2015](https://github.com/lukehoban/es6features) kullanılarak yazılmıştır.

Tüm örnekler doğrudan Vue'nun tam sürümünü kullanarak da derlenebilir. Daha fazla bilgi için [buraya](https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only) göz atın.
:::

Vue.js ve Vue-Router kullanarak tek sayfalı uygulamalar (SPA) oluşturmakşaşırtıcı derecede kolaydır.Vue.js kullanarak, uygulamamızı zaten component'lerden oluşturuyoruz. Vue-router ile yapmanız gereken tek şey, component'i rotaya eşlemek ve hangi adrese işleneceğini bildirmektir. İşte temel bir örnek:

## HTML

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- gezinti için(navigation) router-link component'ini kullanın. -->
    <!-- Linki `to` prop'una belirtin.  -->
    <!-- `<router-link>`  varsayılan olarak `<a>` tag'i olarak görüntülenir -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- Yönlendirme sonu -->
  <!--Geçerli rotayla eşleşen component burada oluşturulur. -->
  <router-view></router-view>
</div>
```

## JavaScript

```js
// 0. Modüler bir sistem kullanıyorsanız (örnegin vue-cli üzerinden),  Vue ve VueRouter'ı içe
// aktarın ve `Vue.use(VueRouter)` ile vue-router'ı projede kullanmak için etkinleştirin.

// 1. Rota component'lerini tanımlayın.
// Bunları diğer dosyalardan da içe aktarabilirsiniz.
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. Rotları tanımlayın
// Her rota bir component ile eşlenmelidir. Component,
// `Vue.extend()` ile oluşturulan bir 'component constructor'
// veya component seçeneklerine sahip bir obje olabilir.
// Daha sonra iç içe geçmiş rotalardan(nested routes) bahsedeceğiz.
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. Bir router instance'ı(router örneği) oluşturun
// ve `routes`  seçeneğini atlayın.
// Burada ek seçenekler verebilirsiniz ama
// şimdilik basit tutalım.
const router = new VueRouter({
  routes // `routes: routes` ifadesinin kısaltılmış hali
})

// 4. Bir root örneği( instance) oluşturun ve bağlayın(mount).
// Uygulamaya router'ı ekleyin, böylece tüm uygulama
// router'ı görebilir ve erişebilir.
const app = new Vue({
  router
}).$mount('#app')

// Uygulama, şimdi hazır!
```

Eklediğimiz router'a, `this.$router` ile erişirken, `this.$route` ile şu anki rotaya, herhangi
bir component içinden ulaşabiliriz:

```js
// Home.vue
export default {
  computed: {
    username() {
      // `params` ifadesinin ne olduğunu ilerde kısaca göreceğiz
      return this.$route.params.username
    }
  },
  methods: {
    goBack() {
      window.history.length > 1 ? this.$router.go(-1) : this.$router.push('/')
    }
  }
}
```

Doküman boyunca `router` instance'ını sıklıkla kullanacağız. `this.$router` ifadesinin `router` ifadesini kullanmak ile aynı olduğunu unutmayın. `this.$router` kullanmamızın nedeni, yönlendirilmesi gereken her component'e bir router import etmek istemediğimizden.

Ayrıca bu [canlı](https://jsfiddle.net/yyx990803/xgrjzsup/) örneğe göz atabilirsiniz.

Hedef rota eşleştiğinde `<router-link>` öğesinin otomatik olarak `.router-link-active` class'ı verildiğine dikkat edin. Daha fazla bilgi edinmek için lütfen [API referansı](../api/#router-link) bakın.
