# Kurulum

## Direk İndirme / CDN

[https://unpkg.com/vue-router/dist/vue-router.js](https://unpkg.com/vue-router/dist/vue-router.js)

<!--email_off-->

[Unpkg.com](https://unpkg.com) npm tabanlı CDN bağlantıları sağlar. Yukarıdaki bağlantı her zaman npm'deki en son sürüme işaret edecektir. `https://unpkg.com/vue-router@2.0.0/dist/vue-router.js` gibi URL'ler aracılığıyla belirli bir sürümü de kullanabilirsiniz.

<!--/email_off-->

Vue'dan sonra `vue-router` eklerseniz, otomatik olarak yüklenir:

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

## npm

```bash
npm install vue-router
```

Modüler bir sistem kullanırken, `Vue.use()` ile açıkca bir router eklemesi yapmanız gerekir :

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

Global script tag'lerini kullanırken bunu yapmanız gerekmez.

## Geliştirme Sürümü

En son geliştirme sürümünü kullanmak istiyorsanız, doğrudan GitHub'dan klonlamanız ve `vue-router`'ı kendiniz oluşturmanız(build) gerekecektir.

```bash
git clone https://github.com/vuejs/vue-router.git node_modules/vue-router
cd node_modules/vue-router
npm install
npm run build
```
