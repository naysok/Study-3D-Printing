# 3D プリントに向けての STL ファイルの修復  

オンラインサービス

- netfabb (Autodesk??)  
- 3D Tools (Microsoft)  

---


### netfabb  

[https://service.netfabb.com/login.php](https://service.netfabb.com/login.php)    

同名の Autodesk の有料サービスがあるが、こっちは無料。  
このサイトからでも、ポリシーのリンクを踏むと Autodesk のサイトに飛ぶので、よくわからない。  

google login で使える。  

~~MacOS からアクセスすると、 microsoft azure が必要なんたらみたいに出て気持ち悪いけど、それは無視していい~~  
出なくなってた（たぶん)  


データによって、アップロードのエラーが出る。  
（アップロードエラーがでたものを、DMM.Make などに投げると、造形可能と帰ってくるので、破損しているとかとはまた違うっぽい）  

手元に破損した都合の良い STL がなかったので、検証できず。。。  


---  

### 3D Tools (Microsoft)  

[https://tools3d.azurewebsites.net/](https://tools3d.azurewebsites.net/)  

かなおかさんに教えてもらった。  

機能として、Repair, Reduce, Convert, 3D Print APIs とか書いてあるので、Reduce をオンラインで処理してくれるなら、便利そう。  

Convert の Converts an STL/OBJ/VRML file to a 3MF file. は、フルカラー 3D プリント向けのフォーマット 3MF のやつかな。  

試しにやってみた repair が終わらないので検証 X です。  


(ここから引用)  
Microsoftは、オートデスク、HP（ヒューレット・パッカード）、Shapeways、ダッソー・システムズ、netFabb、SLMソリューション等と共に、新しいファイルフォーマット「.3MF」開発のため3MFコンソーシアムと呼ばれる共同開発団体を設立しました。  

この3MFコンソーシアムが開発する新しいファイルフォーマット「.3MF」形式は、旧来のCADフォーマット（STL等）のファイルフォーマットとは異なり、3Dモデル、カラー、その他の特性を記述した全ての情報を一つのアーカイブ内に含め、データをPNGやJPEGのように人が認識できるファイルに変換。  
(ここまで引用（[http://idarts.co.jp/3dp/microsoft-3mf-hololens/](http://idarts.co.jp/3dp/microsoft-3mf-hololens/)   より）)  



---  



これらでいう、修復がどういうレベルか本当に気になる  

open mesh → closed mesh （これくらい普通にやってほしい）  

これ、複数の穴の空いた開いたメッシュ、  
例えば、Stanford Bunny の2つの耳の先端が両方、穴が空いてたときとか、そういうのがどうなるのか、  
修復されて変な形になるのが、ちょっと怖い  


invalid mesh → closed mesh （できてくれたら嬉しい）  

普通にできたらめっちゃ助かる  
これもデータの解釈によって、別の形になる可能性ありそう  




---  


あとは、個人的になんども苦労する Rhino でのこと。  

Rhino で、Open Mesh をどう処理するのが正解か結構よくわからない。  
Rhino で出てくるのはこの辺。  

- Open Brep, Open Mesh  
- Invalid Brep, Invalid Mesh (見たことないかな？)  
- Closed Brep, Closed Mesh  


3D プリントするなら、Closed Mesh でなければいけない。  

Open Mesh は、fillHole 系の コマンドか、Brep の段階で、cap/capEx で閉じたい。  

これが、できないことがあるので、ほんとは grasshopper でエッジを探して中点とって、メッシュを構築して、みたいのがあったら良さそう。  
(自分用に、自分で作るモデルがそういうもの多いようだったら作る)  

Invalid のほうは、正直よくわからないが、  
Brep の時は、 Explode して、 Invalid な Surface を特定してそれだけ作り直す、くらいしかわからない。

昔、小野さんに聞いたときにもらったのはこの方法。  
Explode して Join すると、Invalid ナサーフェスだけ残る。  

![photo](src/BrepTest.jpg)  


大事なときに炎上しないように、この辺頑張りたい。  


---  


全然関係ないけど、  
repair する前/した後の STL を git の差分で見ようとしたら、バイナリのなんとかで、見れず。  
github では普通に、3D の preview できた。  
