# Unikernel as Processes
---

## Abstract
* システムの仮想化はマルチテナントクラウドのisolationのデファクトスタンダートとなった
* 更に最近ではunikernelかVMのisolationを再利用する方法として、汎用OSに代わって現れた
	* その代わりunikernelは(libOSとリンクした)アプリケーションを直接仮想ハードウェア上で実行する  
* ここでは、unikernelは実際に仮想ハードウェアを必要せず、しかし同等レベルのisolationを現存するシステムコールを利用して実現できることを示す
* さらにリソースも少なくてよくなる
* 一般的なプロセスのデバッグ・マネジメントツールが使える  

## Introtuction
* unikernelはクラウド環境において他のアプリケーションから切り離してアプリケーションを実行する軽量な方法として現れた
* unikernelは仮想ハードウェア上で動くのに必要なlibOSのパーツとリンクされて成り立ち、VMの望ましいisolationを継承する
* 最初はocamlベースのMirageOSに限られていたが他言語のサポートも増えてきた
* 軽量で協力なisolationを活かして組み込み、ネットワーク上の仮想化、サーバーレス・コンピューティングに適している
* しかしVMの特徴はいくつかの問題がある
	* 既存のVMのtoolchainは軽量なものリアルタイム性が求められるクラウドサービスを動かすために設計されていないのでunikernelあるいは軽量なVMの起動時間、メモリ効率、パフォーマンスのために再設計が必要
	* デバッグの手段
	* unikernelはすでに仮想化されたインフラの上には配置できない
* unikernelをプロセスとして動かすと
	* プロセスやコンテナのの軽量なツールや標準のプロセスのデバッグツールが使える
	* すでに仮想化されたインフラの上で動かせる
* これは二つの発見に基づいている
	* unikernelはどちらかというとkernelよりはアプリケーションに近いのでスーパーバイザーレベルの仮想化はいらない(x86におけるring0など)
	* もしVMのためのisolationがホストへのアクセスをハイパーコールインターフェースによって制限することで実現しているなら、同じことは既存のサンドボックス化ツールを用いてシステムコールを制限することでプロセスでも実現できる(seccomp)

## Conclusion
* unikernelにもkernel-likeなコードは存在する
	* isolationのためにVMとして実行するためのコード
* unikernelをプロセスとして実行することはVMより良いisolationを実現できる（？）
* unikernelをプロセスとして実行することはunikernelをメインストリームへ
	* プロセスの特徴を受け継ぐ(メモリの利用効率、パフォーマンス、起動時間...etc)