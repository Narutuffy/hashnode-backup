---
title: "Clearing Cookie in Next.js"
seoTitle: "Clearing Cookie in Next.js from Server Side"
datePublished: Thu Feb 03 2022 10:47:53 GMT+0000 (Coordinated Universal Time)
cuid: ckz6uuiux00000ynvg14u5jjz
slug: clearing-cookie-in-nextjs
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/eSFEvmcIu2w/upload/v1643877018230/Y05xuutiR.jpeg
tags: nodejs, nextjs

---

Like any other header, a cookie is also a header that web apps use to store essential information. If you want to clear a cookie from server-side in Next.js (which uses node.js under the hood) you can simply use response's `setHeader` method like so:

```js
res.setHeader('Set-Cookie', 'some-cookie=someValue; Max-Age=0');

``` 
The important part is setting `Max-Age=0`, which will make the browser delete that cookie immediately.

___
PS: This will work for other frameworks and languages too, syntax might be different but Headers work in the same way across all browsers