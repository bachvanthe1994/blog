---
layout: post
title:  "[android] Multi screen use smallestWidth"
date: 2020-06-26 16:33:23 +0700
categories: [android]
---

<img src="/static/img/posts/sdp_example.png" style="width: 400px;">



**So what is smallestWidth ?**
<br/>
Theo 1 cách hiểu đơn giản nhất đó là:

Mỗi màn hình điện thoại sẽ ứng với 1 thư mục swdp khác nhau. Hệ thống sẽ tự select vào thư mục resource đó. Bằng cách lấy giá trị nhỏ nhất giữa chiều dài và chiều rộng của máy (đơn vị dp). Sau khi tính xong sẽ so sánh với các thư mục trong resource, nếu giá trị đó gần nhất với nào (theo chiều giảm dần, nếu min = 360dp thì nó sẽ chỉ check các thư mục có nhỏ hơn hoạc bằng 360dp) thì sẽ chọn thư mục đó làm nguồn.

Vd ta có:

``` xml
res/layout-sw480dp/
res/layout-sw320dp/
res/layout-sw300dp/
```

Nếu giá trị mà hệ thống tính được

min = 320dp, vậy hệ thống sẽ chọn res/layout-sw320dp/
<br/>
min = 470dp, vậy hệ thống sẽ chọn res/layout-sw320dp/
<br/>
min = 490dp, vậy hệ thống sẽ chọn res/layout-sw480dp/
<br/>
Vậy làm sao biết màn hình này nó sẽ lấy thư mục nào ?

Ta sẽ phải tính min(height, width) *tức là gia trị nhỏ nhất trong height và width*. Sau khi tính xong, ta sẽ quy đổi nó sang giá trị là dp. Và sau khi tính xong bạn đã biết màn hình này sẽ phù hợp với thư mục nào rồi đó.

Tại sao lại dùng cách này để multi screen ?

Vì mỗi màn hình lại có mật độ dp khác nhau *1dp = ? px theo từng màn hình, từ đó ta sẽ biết được màn hình đó có giá trị width và height bằng bao nhiêu dp*

Vd: ta có các màn hình có tỉ lệ width:height (giá trị là dp) là:

A: 320×533(dp)
B: 360×592(dp)

Và bạn có 1 layout có width = 320dp

``` xml
<LinearLayout
   android:layout_width="320dp"
   android:layout_height="100dp"></LinearLayout>
```

Tất nhiên khi chạy máy A thì nó sẽ đổ đầy chiều with của màn hình, tuy nhiên khi sang máy B thì nó lại bỉ thừ 1 khoảng là 360 – 320 = 40dp !-__-)

Vậy để khác phục điều này ta sẽ tạo ra 2 thư mục đó là:

``` xml
res/values-sw320dp/
res/values-sw360dp/
```

Sau đó ở 2 thư mục tạo 1 file dimens.xml

``` xml
res/values-sw320dp/dimens.xml
res/values-sw360dp/dimens.xml
```

Ta sẽ tạo 1 key ở trong file dimens.xml

Với thư mục:

``` xml
sw320dp: <dimen name="size_layout">320dp</dimen>
sw360dp: <dimen name="size_layout">360dp</dimen>
```

Ta sẽ sửa file layout để sử dụng theo theo resource @dimen

``` xml
<LinearLayout
   android:layout_width="@dimen/size_layout"
   android:layout_height="100dp"></LinearLayout>
```

Và giờ hãy xem 2 màn hình:

Qua bài viết này hy vọng các bạn hiểu được phần nào làm sao để support multi screen sử dụng cách mà bài viết đã nêu. Thanks

Chia sẻ 1 project mà 1 user đã tính cho chúng ta rồi, hỗ trợ rất nhiều các màn hình

[https://github.com/intuit/sdp](https://github.com/intuit/sdp)