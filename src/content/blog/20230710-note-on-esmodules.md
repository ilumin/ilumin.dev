---
title: Note on ES modules
description: บันทึกกันลืม ES modules คืออะไร
postSlug: note-on-es-modules
draft: false
pubDatetime: 2023-07-10
tags: 
  - note
  - javascript
---

เวลาที่เราเขียนโปรแกรม เรามักจะจัดการ code ของเราโดยแบ่งไฟล์ตามการใช้งาน หรือที่เค้าเรียกว่า responsibility
ถ้าเป็นในภาษาอื่นๆ เช่น Java เค้าใช้ `namespace` ในการแบ่ง ซึ่งจะสามารถเรียกใช้ class ภายใต้ `namespace` ที่ต้องการได้

แต่สำหรับ Javascript หรือ Node.js เราเรียกของเหล่านี้ว่า module ซึ่งเราสามารถดึงมันมาใช้งานได้ 2 แบบคือ

1. CommonJS modules
2. ES modules

> **Note**
> พอดีไปทำ survey ของ JetBrains แล้วเจอคำถามเกี่ยวกับ ES modules ก็เลยมาจดบันทึกกันลืมสักหน่อย
> ลองเข้าไปทำกันนะ [ที่นี่](https://surveys.jetbrains.com/s3/developer-ecosystem-survey-2023-sh?pcode=7260701625215918)

## CommonJS modules

เว่ากันซื่อๆ คือการใช้ `require` เพื่อดึง และใช้ `module.exports` เพื่อสร้าง module

หน้าตาก็จะเป็นแบบนี้

```js
// show-me-the-money.js
const ShowMeTheMoney = () => throw new Error(`Sorry, I'm broke :(`)

module.exports = ShowMeTheMoney

// main.js
const ShowMeTheMoney = require('./show-me-the-money')

ShowMeTheMoney()
```

นี่เป็นวิธีปกติที่ Node.js จะดึง module มาใช้งาน นั่นคือเวลารัน Node.js โดยที่ไม่ได้ setup อะไรเลยก็จะรันได้ทันที

## ES modules

> ชื่อยาวๆคือ ECMAScript modules เรียกแบบย่อๆคือ ESM หรือจะเอาให้งงกว่านั้นเค้าจะเรียกว่า ES6 modules

คือการใช้ `import` และ `export` นั่นเอง

หน้าตาก็จะเป็นแบบนี้

```js
// show-me-the-money.js
const ShowMeTheMoney = () => throw new Error(`Sorry, I'm broke :(`)

export default ShowMeTheMoney

// main.js
import ShowMeTheMoney from './show-me-the-money'

ShowMeTheMoney()
```

ในโปรเจคถ้าหากว่าจะใช้ ES modules สามารถทำได้ 2 วิธี

1. เปลี่ยนนามสกุลไฟล์จาก `.js` เป็น `.mjs` จะสามารถใช้งานได้ทันที หรือ
2. แก้ `"type": "module"` ในไฟล์​ `package.json` (และแน่นอนว่าถ้าไม่ใส่หรือใส่เป็น `commonjs` มันก็จะใช้ CommonJS นั่นเอง)

## ความสับสน

- **CommonJS** เป็น default ของ Node.js (หรือฝั่ง server-side)  
  และมาก่อน ES modules นั่นทำให้หลายๆ package ใช้ commonjs รวมทั้ง framework ฝั่ง client-side บางตัวจะใช้วิธีการแปลงโค้ด(transpiler) จาก `import/export` มาเป็น `require/module.exports`
- **ES modules** เป็นมาตรฐานของ Javascript (หรือฝั่ง client-side)  
  นั่นทำให้ package ฝั่งหน้าบ้านใช้ `import/export` ซะเยอะ

## ควรใช้อันไหน

- จากโค้ดตัวอย่าง การใช้ ES modules เนี่ยทำให้เราสามารถแยกโค้ดได้อย่างชัดเจน ว่าส่วนไหนคือการ define variable หรือการดึงเอา module มาใช้งาน
- รายละเอียดที่ลึกกว่านั้นคือ ES modules จะดึงแบบ asynchronous ในขณะที่ CommonJS ดึงแบบ synchronous นั่นหมายความว่า ES modules น่าจะดึง module ได้เร็วกว่า CommonJS

สำหรับผมแล้ว 2 ประเด็นนี้ทำให้ ES modules น่าใช้งานมากกว่า
