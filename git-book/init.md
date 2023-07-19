# GitBook 요소 참고 템플릿

## 링크블럭 만들기

{% content-ref url="{{링크주소}}" %}
[{링크이름}]({{링크주소}})
{% endcontent-ref %}

## 카드블럭 만들기

<table data-view="cards">
  <thead>
    <tr>
      <th></th>
      <th data-hidden data-card-target data-type="content-ref"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1번카드</td>
      <td><a href="{1번카드주소}">1번카드.md</a></td>
    </tr>
    <tr>
      <td>2번카드</td>
      <td><a href="{2번카드주소}">2번카드.md</a></td>
    </tr>
    <tr>
      <td>3번카드</td>
      <td><a href="{3번카드주소}">3번카드</a></td>
    </tr>
  </tbody>
</table>

## 힌트 만들기

{% hint style="info" %}
style에는 success, info, warning, danger가 있습니다.
{% endhint %}

## 아코디언 만들기

<details>

<summary>아코디언 제목</summary>

아코디언 내용

</details>

## 구분선 만들기

***

## 인용구 만들기

> 인용구 내용
