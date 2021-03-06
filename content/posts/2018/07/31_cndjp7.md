---
title: "cndjp第7回勉強会でLTしてきました"
date: 2018-07-31T23:05:50+09:00
categories: ["infra", "mackerel", "kubernetes"]
draft: false
aliases: ["/post/20180931_cndjp7/"]
---

[cndjp](https://cnd.connpass.com/event/93986/)でkubernetes環境のmackerelでの監視についてLTしてきました。


<iframe src="//www.slideshare.net/slideshow/embed_code/key/aqYuLn1At1EeCY" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/koooge/mackerel-on-k8s-107695015" title="Mackerel on k8s" target="_blank">Mackerel on k8s</a> </strong> from <strong><a href="https://www.slideshare.net/koooge" target="_blank">koooge</a></strong> </div>

Mackerelは、はてな社が作っているマネージドのサーバー監視のサービスで直訳すると鯖です。

表紙はそのロゴをgoogle翻訳のカメラモードで撮った時のスクショ。これ、やってみたら分かりますが、Google翻訳が左のMっぽいマークも含めてサバに翻訳しようとするので結構難しいです。
苦労したのでやり方を共有すると、Google翻訳をカメラモードで構え、Mackerelの文字を右から左に迫っていきロゴを除いたMackerelの部分だけサバに翻訳された瞬間にスクショしてください。0.5秒くらいでロゴのM部分を含めてサバに変わるので注意です。これだとMMackerelになるのでサバではないと思うんですんが、Google翻訳の忖度によってMを含めてサバになりました。私はこの画像の撮影に20秒かける予定で15分かけました。


以上。

---

### kubernetes
説明不要と思いますがgoogleが作ったコンテナのオーケストレーションツール。
昨今swarmは息絶え、ECSやACSはベンダーロックイン上等な人しか使えず、とりあえずkubernetesだね？という風潮があります。良し悪しあると思いますが実態としてkubernetesはデファクトスタンダードであり、避けては通れない存在となりました。

### Mackerel vs Datadog
Mackerelは冒頭で書きましたが、古来より日本の凄腕エンジニアが吸い込まれていった（ている）はてな社が作ってるさいつよのサーバ管理監視サービス。
一方DataDogは、近年これまた別の凄腕エンジニアを吸い込んでいっているM社やC社が使っている影響で、マネージドのサーバー監視サービスとえばDataDogみたいになっています。

が、使い方や置かれている体制次第ではMackerelが良いと思ってます。

基本機能に関しては、もはやそこまで差異がないと思います。(監視ができ、通知のインテグレーションが豊富であり、年単位でデータを保存してくれる)

DataDog比較してMackerelの最も良い点は、日本語英語両方の公式情報とサポートです。チーム全員の英語力が高ければ障害になりませんが、実際にはそんなチームは限られていると思います。また、サービスの更新情報も英語だと読めるけど疲れるので読み飛ばしませんか、私は読み飛ばします。

逆にまだまだこれからな点はkubernetes対応です。自分でplugin作れという話もありますが、それならたいていの場合DataDog使うと思います。

### Mackerel vs Prometheus
PrometheusはCNCFでホストされていることもあり、k8sと組み合わせるなら一番良さそうです。
が、小規模チームではPrometheusとかGrafanaを運用するのはパワーいるので難しい。

### NewRelicとか
私の観測範囲が偏っているのか最近聞かないですね？

### ほか
非マネージドで何か使うならPrometheus一択と思われます。マネージドはどれとってもロックインですので、突飛なサービスは宗教的な理由がない限り選択したくないですね。そろそろGCPとかAWSあたりにマネージドPrometheusが出ないのかな？

## まとめ
- 小規模チームで英語onlyじゃつらい環境でkubernetes使ってないならMackerel
- 小規模チームでkubernetes使ってる場合は、英語頑張って(頑張ってもらって)DataDogにか、kubernetes対応頑張ってMackerel
- 大規模チームは、マンパワーがあるので何でもできると思いますが、いろんなしがらみや制約があると思うので頑張ってください！
