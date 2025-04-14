.. _licensesetupdate:

================================================
ライセンスの設定・更新
================================================

.. note::

   ニューラルネットワークの学習を行う機能（\ :option:`sannp --train`\ 、\ :option:`sannp --train-charge`\ 、\ :option:`sannp --classical`\ ）を使う場合に、有効なライセンスが必要となります。その他の機能、およびQuantum ESPRESSO・LAMMPSを実行する際には、ライセンスは不要です。slhmc（自己学習ハイブリッドモンテカルロ法の実行ファイル）は、それ自身の実行にはライセンスは不要ですが、sannpを呼び出してニューラルネットワークの学習を行うためにライセンスが必要です。

- NeuralMDをインストールしているマシンでGUIアプリケーションが使用できる場合:

  以下の\ :ref:`licenseset`\ または\ :ref:`licenseupdate`\ を参照してください。

  .. image:: /img/license_local.svg
     :height: 200 px

- NeuralMDをインストールしているマシンでGUIアプリケーションが使用できない場合:

  以下の\ :ref:`remoteACC`\ を行った後、ACCのアドレスを読み替えて\ :ref:`licenseset`\ または\ :ref:`licenseupdate`\ の手順を実行してください。

  .. image:: /img/license_remote.svg
     :height: 200 px

- フローティングライセンスをご購入の場合は\ :ref:`floating`\ を参照してから上記の手順を実行してください。

  .. image:: /img/license_floating.svg
     :height: 200 px

.. note::

      NeuralMDをインストールしたマシンがインターネットに接続されていない場合、別途インターネットに接続されているマシンが必要です。

.. _licenseset:

ライセンスの設定
=============================

ライセンス登録後、noreply\@sentinelcloud.comからEntitlement Certificateをメールでお送りします。Entitlement Certificateに記載されているProduct Keyを用いてライセンスの設定を行います。

.. _licenseaccc2v:

C2Vファイルの生成
+++++++++++++++++

NeuralMDをインストールしているマシンのウェブブラウザで、 Admin Control Center (ACC) (http://localhost:1947) にアクセスします。

.. note::

     ACCはオフラインのマシンからもアクセス可能です。

ACCのSentinel Keys画面のリストのうち、Vendorの欄に32462と記載されている行の、Fingerprintボタンをクリックして、C2Vファイル（fingerprint_32462.c2v）をダウンロードします。

.. image:: /img/ACCSentinelKeys.png

.. note::

    弊社の他の製品のキーがマシンに対して登録されている場合、Vendorの欄に32462と記載されている行にFingerprintボタンは表示されません。この場合は、弊社の他の製品のキーに表示されているC2Vボタンをクリックして、C2Vファイル（(KeyID)_(timestamp).c2v）をダウンロードしてください。

.. note::

      ライセンスの更新をする際は、更新を適用するキーに表示されているC2VボタンをクリックしてC2Vファイル((KeyID)_(timestamp).c2v)をダウンロードしてください。

.. _licenseaccv2cpl:

V2CPファイルの生成
+++++++++++++++++++

次に、ウェブブラウザで\ `Entitlement Management System (EMS) <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスします。

.. note::
      
      NeuralMDをインストールしたマシンがオフラインの場合は、ダウンロードしたC2Vファイルをオンラインの別のマシンに移動したうえで\ `EMS <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスしてください。


"Product Key ID"の入力欄に、Entitlement Certificateに記載されている"Product Key"を入力してログインボタンをクリックしてください。

.. image:: /img/EMSLogin.png

Products画面が開いたら、Activate Offlineボタンをクリックします。

.. image:: /img/EMSProducts.png

Activate Products画面が開いたら、Select Fileボタンをクリックして、先ほどダウンロードしたC2Vファイルを選択し、Complete Activationボタンをクリックします。

.. note::

      初めてSentinelライセンスの設定を行う場合に必要なC2Vファイルのファイル名はfingerprint_32462.c2vですが、更新の際に必要なC2Vファイルのファイル名は(KeyID)_(timestamp).c2vとなります。

.. image:: /img/EMSActivateProductsFingerprint.png

アクティベーションに成功したら、Download Licenseをクリックして、V2CPファイル(拡張子:.v2cp)をダウンロードします。

.. image:: /img/EMSActivatedFingerprint.png

.. note::

      同一のV2CPファイルを圧縮して添付したメールが自動配信されますので、そちらを解凍してご利用いただくことも可能です。

.. _licenseaccv2cpapplyl:

V2CPファイルの適用
+++++++++++++++++++

ACCの画面に戻り、左側のメニューからUpdate/Attach画面を開きます。Select Fileボタンから、ダウンロードしたV2CPファイルを選択し、Apply Fileボタンをクリックしてください。

.. note::
      
      C2Vファイルをオンラインの別のマシンに移動してV2CPファイルを生成した場合は、NeuralMDをインストールしたマシンにV2CPファイルを移動したうえでACCにアクセスしてください。

.. image:: /img/ACCApply.png

V2CPファイルのApplyに成功したら、ライセンスの設定は完了となります。

.. _remoteACC:

リモートのACCへのアクセス設定
=================================

NeuralMDをインストールしたマシンでGUIアプリケーションが使用できない場合、GUIアプリケーションを使用可能で同一ネットワーク上にある別のマシンからACCにアクセスする必要があります。

そのためには、以下の方法で、ACCへのリモートアクセスの許可設定を行ってください。

- NeuralMDをインストールしたマシンの/etc/hasplm/にhasplm.iniファイルを作成し、以下の例を参考にして設定を記述してください。

 .. table::

     +-------------------------------------------------------------------------------------------+
     |GUIアプリケーションを使用可能なPCのIPアドレスが192.168.00.000の場合                        |
     +===========================================================================================+
     || [SERVER]                                                                                 |
     || accremote = 1                                                                            |
     || [ACCESS]                                                                                 |
     || allow = 192.168.00.000                                                                   |
     || deny = all                                                                               |
     +-------------------------------------------------------------------------------------------+

 .. note::

      allow行は複数記述可能です。ただし、deny = all 行よりも後に記述したallow行は無効となりますのでご注意ください。

 .. note::

      上記のアクセス設定は、ACCだけでなくライセンス本体にも適用されます。

      フローティングライセンスにクライアントマシンからアクセスできない場合は、allow行にクライアントマシンのIPアドレスを追加してください。

 .. note::

      deny = all 行を記述することにより、allow行に記述されていないマシンからの不正なアクセスを防ぐことができます。また、ACCに接続後は、GUI画面からパスワードを設定してアクセス権限を管理することも可能です。

以上の設定を行うと、allow行にIPアドレスを指定したPCのウェブブラウザのアドレス欄にhttp://<NeuralMDをインストールしているマシンのIPアドレス>:1947と入力することで、NeuralMDをインストールしているマシンのACCにアクセスできます。

.. _floating:

フローティングライセンスの設定
===============================

フローティングライセンスをご購入いただいた場合は、ライセンスの設定を行ったマシンと同一のネットワーク上にある別のマシン（Windows・Linux）でもNeuralMDを使うことができます。

- ライセンスサーバーとして使うマシン側

 `Sentinel RTE（ライセンスマネージャー）をダウンロード <https://apps.advancesoft.jp/sentinel/Sentinel-LDK-RTE-for-AdvanceSoft-v9.15_Linux.tar.gz>`_\ ・インストールし、ACCを利用してライセンスの設定を行ってください。

 マシンの起動時に毎回自動でライセンスマネージャが起動するため、一度ライセンスの設定を行って以降は特に必要な操作はありません。

- NeuralMDを使うクライアントマシン側

 NeuralMDをインストールして下さい。通常、NeuralMDをインストールするだけで計算は実行可能となりますが、ライセンスエラーが発生する場合は、次のいずれかの方法でライセンスサーバーのIPアドレスを設定する必要があります。

 - Sentinel RTEをインストール済みで、GUIアプリケーションが使用できる場合 :
  
   クライアントマシンから\ `Admin Control Center (ACC) <http://localhost:1947>`_\ にアクセスし、画面左側のメニューからConfiguration画面を開いてください。次に、Access to Remote License Managersタブを開いて、Remote License Search ParametersにライセンスサーバーのIPアドレスを入力し、Submitをクリックしてください。
  
 - Sentinel RTEをインストール済みで、GUIアプリケーションが使用できない場合 :

   /etc/hasplm/にhasplm.iniファイルを作成し、以下の例を参考にしてライセンスサーバーのIPアドレスを記述してください。

 - Sentinel RTEをインストールできない場合 :

   以下のディレクトリにhasp_32462.iniファイルを作成し、例を参考にしてライセンスサーバーのIPアドレスを記述してください。
  
   - Windowsの場合 : %LocalAppData%\\SafeNet Sentinel\\Sentinel LDK\\

   - Linuxの場合 : $HOME/.hasplm/
  
  .. table::
 
      +-------------------------------------------------------------------------------------------+
      |IPアドレスが192.168.00.000の場合のiniファイルへの記述例                                    |
      +===========================================================================================+
      || [REMOTE]                                                                                 |
      || serveraddr = 192.168.00.000                                                              |
      +-------------------------------------------------------------------------------------------+ 

.. note::

  ライセンスサーバーのファイアウォールの設定で、TCP/UDP ポート 1947が開放されていない場合は、設定を変更して開放してください。（Windowsマシンの場合、インストール時に自動でこれらのポートは開放されるため、通常ではファイアウォールの設定は必要ありません。）

.. _licenseupdate:

ライセンスの更新
=========================
support.nano@advancesoft.jp :sup:`*` にライセンスの更新をリクエストしてください。

.. role:: smallnote
   :class: small-note

:smallnote:`* このメールアドレスへの特定電子メール（広告・宣伝メール）の送信を拒否いたします。`

ライセンス登録後、noreply\@sentinelcloud.comから新しいEntitlement Certificateをメールでお送りしますので、記載されているProduct Keyを用いてライセンスの更新を行ってください。

基本的な操作方法は\ :ref:`licenseset`\ と同様です。ただし、以下の点に注意してください。

- C2VファイルおよびV2CPファイルは必ず新たに生成したものを使用してください。過去の設定・更新時に生成したものを誤って使用しないようにご注意ください。

- ACCのSentinel Keys画面からC2Vファイルをダウンロードする際は、必ず、更新を適用するキーに表示されているC2Vボタンをクリックしてダウンロードを行ってください。
  
.. note::

      初めてSentinelライセンスの設定を行う場合に必要なC2Vファイルのファイル名はfingerprint_32462.c2vですが、更新の際に必要なC2Vファイルのファイル名は(KeyID)_(timestamp).c2vとなります。
