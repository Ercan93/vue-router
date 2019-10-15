# Adlandırılmış Rotalar

Bazen, özellikle bir rotaya bağlanırken veya navigasyonlar yaparken adı olan bir rota tanımlamak daha uygundur. Router instance'ı oluştururken `routes` seçeneğinin içerisinde rotayı adlandırabiliriz:

``` js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

Adlandırılmış bir rotaya bağlanmak için bir objeyi `router-link` bileşenin `to` prop'ına iletebilirsiniz:

``` html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

Aslında bu programlama açısından `router.push()` ile aynı şeydir:

``` js
router.push({ name: 'user', params: { userId: 123 }})
```

Her iki durumda da, router `/user/123` yoluna gidecektir.

Tam bir örnek için [buraya](https://github.com/vuejs/vue-router/blob/dev/examples/named-routes/app.js) bir göz atabilirsiniz.
