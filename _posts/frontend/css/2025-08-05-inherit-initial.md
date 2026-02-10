---
title: inherit, initial
description: 자동완성 목록을 보면 꼭 보이는 그것들
date: 2025-08-05 15:10:00 +0900
categories: [프론트엔드, CSS]
---

css 쓰다보면 꼭 이 둘이 보인다.
![Desktop View](https://lh3.googleusercontent.com/pw/AP1GczMvtlQbVXCi6pcjVCnjAiTNlWe1Ja_oyspj74nprxBP4K_BOvn4wkXB1GcPDc6vHBZZtyz_GUQ6MoOca8NJQ4wJ9voCYr_DLbI9G68HruGJJT7Pca8=w2400){: .w-75 }
_둘 다 말로 풀어쓰면 inherit는 **상속**, initial은 **기초값으로의 초기화**를 의미한다._
<br>

## inherit 예제
```html
<div class="black">
    <div class="red"></div>
    <div class="green"></div>
    <div class="blue"></div>
</div>
```
{: file='HTML 예제 코드' }

```css
div {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 500px;
    height: 500px;
    background-color: black;
}

div>div {
    width: 50px;
    height: 50px;
    border: 1px solid #fff;
}

.red {
    background-color: red;
}

.green {
    background-color: green;
}

.blue {
    background-color: inherit;
}
```
{: file='CSS 예제 코드' }

![Desktop View](https://lh3.googleusercontent.com/pw/AP1GczMink8YQLmwlf4Bc1vkpWQpa-SEvPAcf_mI2NZSahkOMz28GYSgIN8vK84_2EdZ4bLHb6_3R48NWj8XOXEtTGYUvti46cFxm1bF_oQd5tRUXGy4XfE=w2400){: .w-75 }
_blue의 색상은 상속을 받기에 부모 div의 색상인 black을 사용_


## initial 예제
```html
    <h1>Initial Test!!</h1>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Dicta quae repellat minus ducimus, labore assumenda
        beatae accusamus porro, sed nobis voluptates. Molestias aperiam iusto aut laboriosam blanditiis temporibus
        libero placeat.</p>
```

```css
* { color: green; }

h1 { color: initial; }
```

![Desktop View](https://lh3.googleusercontent.com/pw/AP1GczPvXpyrGrXLI_SX0T5ruuveMoD2S1dZERnrq2C2oJjIhkFnYWsJjRBvQE_WzfHTltrh3WMdmUrUcpktbniqT0QfIA-Jl8_8aJu7PPBW7eUkGt4W4dk=w2400){: .w-75 }
_원래라면 green이 되어야 하는데, 기초값인 black이 설정되었다._