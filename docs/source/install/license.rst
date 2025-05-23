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

  以下の\ :ref:`remoteACC`\ を行った後、GUIアプリケーションを使用可能な別のマシンから\ :ref:`licenseset`\ または\ :ref:`licenseupdate`\ の手順を実行してください。

  .. image:: /img/license_remote.svg
     :height: 200 px

- フローティングライセンスをご購入の場合は\ :ref:`floating`\ を参照してから上記の手順を実行してください。

  .. image:: /img/license_floating.svg
     :height: 200 px

.. _licenseset:

ライセンスの設定
=============================

ライセンス登録後、noreply\@sentinelcloud.comからEntitlement Certificateをメールでお送りします。Entitlement Certificateに記載されているProduct Keyを用いてライセンスの設定を行います。

.. _licenseaccc2v:

C2Vファイルの生成
+++++++++++++++++

Sentinel-LDK-RTEをインストールしているマシンのウェブブラウザで、Admin Control Center (ACC) (http://localhost:1947) にアクセスします。ACCはオフラインのマシンからもアクセス可能です。

.. note::

      Sentinel-LDK-RTEはNeuralMDインストーラの「Sentinel-LDK-RTEをインストールしますか？」ではいを選択した場合、NeuralMDと同時にインストールされます。

.. note::

      ウェブブラウザが使用できない場合は、\ :ref:`remoteACC`\ を参照して、ウェブブラウザを使用可能な別のマシンからACCにアクセスしてください。

ACCのSentinel Keys画面のリストのうち、Vendorの欄に32462と記載されている行の、Fingerprintボタンをクリックして、C2Vファイル :file:`fingerprint_32462.c2v` をダウンロードします。

.. image:: /img/ACCSentinelKeys.png

.. note::

    弊社の他の製品のキーがマシンに対して登録されている場合、Vendorの欄に32462と記載されている行にFingerprintボタンは表示されません。この場合は、弊社の他の製品のキーに表示されているC2Vボタンをクリックして、C2Vファイル :file:`(KeyID)_(timestamp).c2v` をダウンロードしてください。

.. warning::

      ライセンスの更新をする場合は、必ず、更新を適用するキーに表示されているC2VボタンをクリックしてC2Vファイル :file:`(KeyID)_(timestamp).c2v` をダウンロードしてください。

.. _licenseaccv2cpl:

V2CPファイルの生成
+++++++++++++++++++

次に、ウェブブラウザで\ `Entitlement Management System (EMS) <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスします。

.. note::

      C2Vファイルをダウンロードしたマシンがオフラインの場合は、C2Vファイルをオンラインの別のマシンに移動したうえで\ `EMS <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスしてください。

"Product Key ID"の入力欄に、Entitlement Certificateに記載されている"Product Key"を入力してログインボタンをクリックしてください。

.. image:: /img/EMSLogin.png

Products画面が開いたら、Activate Offlineボタンをクリックします。

.. image:: /img/EMSProducts.png

Activate Products画面が開いたら、Select Fileボタンをクリックして、先ほどダウンロードしたC2Vファイルを選択し、Complete Activationボタンをクリックします。

.. warning::

      ライセンスを更新する場合は、 :file:`fingerprint_32462.c2v` ではなく、必ず、手前の手順でダウンロードしたC2Vファイル :file:`(KeyID)_(timestamp).c2v` を使用してください。

.. image:: /img/EMSActivateProductsFingerprint.png

アクティベーションに成功したら、Download Licenseをクリックして、V2CPファイル（拡張子:.v2cp）をダウンロードします。

.. image:: /img/EMSActivatedFingerprint.png

.. note::

      同一のV2CPファイルを圧縮して添付したメールが自動配信されますので、そちらを解凍してご利用いただくことも可能です。

ダウンロードしたV2CPファイルは、Sentinel-LDK-RTEをインストールしたマシンの任意のディレクトリに格納してください。

.. _licenseaccv2cpapplyl:

V2CPファイルの適用
+++++++++++++++++++

ACCの画面に戻り、左側のメニューからUpdate/Attach画面を開きます。Select Fileボタンから、ダウンロードしたV2CPファイルを選択し、Apply Fileボタンをクリックしてください。

.. image:: /img/ACCApply.png

V2CPファイルのApplyに成功したら、ライセンスの設定は完了となります。

.. _remoteACC:

リモートのACCへのアクセス設定
=================================

Sentinel-LDK-RTEをインストールしたマシンでGUIアプリケーションが使用できない場合、GUIアプリケーションを使用可能かつネットワーク接続された別のマシンからACCにアクセスする必要があります。

.. note::

      Sentinel-LDK-RTEはNeuralMDインストーラの「Sentinel-LDK-RTEをインストールしますか？」ではいを選択した場合、NeuralMDと同時にインストールされます。

そのためには、以下のいずれかの方法で、ACCへのリモートアクセス設定を行ってください。

- Sentinel-LDK-RTEをインストールしたマシンの :file:`/etc/hasplm/hasplm.ini` ファイルを管理者権限で編集し、以下の例を参考にして設定を記述してください。

 .. table::

       +-------------------------------------------------------------------------------------------+
       |/etc/hasplm/hasplm.iniの設定例                                                             |
       +===========================================================================================+
       || accremote = 1                                                                            |
       || adminremote = 0                                                                          |
       +-------------------------------------------------------------------------------------------+

 .. warning::

      adminremoteに別の値が既に設定されている場合は、その値を変更する必要はありません。accremoteの値のみを変更してください。

 以上の設定を行うと、別のマシンのウェブブラウザのアドレス欄に http\\ ://<Sentinel-LDK-RTEをインストールしたマシンのIPアドレス>:1947 と入力することで、Sentinel-LDK-RTEをインストールしたマシンのACCにアクセスできます。

 .. note::

      リモートのACCに接続後、GUI画面からパスワードを設定することを推奨します。
      ACCの画面左側のConfigurationを選択し、Basic Settingsタブを開くと、Password Protectionの欄からパスワードの適用範囲とパスワードを設定できます。
      このパスワードは、選択した適用範囲(ACCの設定ページまたは全てのページ)にアクセスする際に必要となります。


- SSHポートフォワーディングを利用してリモートのACCへアクセスします。詳細については\ `こちらのドキュメント <https://apps.advancesoft.jp/sentinel/doc/index.html>`_\ を参照してください。

.. _floating:

フローティングライセンスの設定
===============================

フローティングライセンスをご購入いただいた場合は、ライセンスの設定を行ったマシンとネットワーク接続された別のマシン（Windows・Linux）でもNeuralMDを使うことができます。

- ライセンスサーバーとして使うマシン側

 `Sentinel-LDK-RTE（ライセンスマネージャー）をダウンロード <https://apps.advancesoft.jp/sentinel/Sentinel-LDK-RTE-for-AdvanceSoft-v10.13.1_Linux.tar.gz>`_\ ・インストールしてください。

 .. code-block:: console

      tar -xf Sentinel-LDK-RTE-for-AdvanceSoft-v10.13.1_Linux.tar.gz
      cd aksusbd-10.13.1
      sudo ./dinst
      # アンインストール時には、同フォルダのdunstを実行してください。
      sudo ./dunst

 インストール後、ACCを利用してライセンスの設定を行ってください。

 マシンの起動時に毎回自動でライセンスマネージャーが起動するため、一度ライセンスの設定を行って以降は特に必要な操作はありません。
 
 .. note::

      ライセンスサーバーでGUIアプリケーションを使用できない場合は\ :ref:`remoteACC`\ を行った後、GUIアプリケーションを使用可能な別のマシンからACCにアクセスしてライセンスの設定を行ってください。

 .. note::

      Windowsマシンをライセンスサーバーとする場合は、 `こちらのSentinel-LDK-RTE（ライセンスマネージャー）をダウンロード <https://apps.advancesoft.jp/sentinel/Sentinel-LDK-RTE-for-AdvanceSoft-v10.13.1_Windows.exe>`_\ し、\ `こちらのドキュメント <https://apps.advancesoft.jp/sentinel/doc/index.html>`_\ を参照してインストールを行ってください。

 .. note::

       ファイアウォールの設定で、TCP/UDP ポート 1947が開放されていない場合は、設定を変更して開放してください。（Windowsマシンの場合、インストール時に自動でこれらのポートは開放されるため、通常ではファイアウォールの設定は必要ありません。）

- NeuralMDを使うクライアントマシン側

 NeuralMDをインストールして下さい。通常、NeuralMDをインストールするだけで計算は実行可能となります。

 .. note::

      ライセンスサーバーと異なるネットワークセグメントに存在するクライアントマシンで、NeuralMDインストール時にライセンスサーバーのIPアドレスまたはホスト名を設定していない場合は、次の方法で設定を行ってください。

      以下のディレクトリに :file:`hasp_32462.ini` ファイルを作成（既にある場合は編集）し、以下の例を参考にしてライセンスサーバーのIPアドレスまたはホスト名を記述（既にある場合は行を追加）してください。

      - Windowsの場合 : :file:`%LocalAppData%\\SafeNet Sentinel\\Sentinel LDK\\`

      - Linuxの場合 : :file:`$HOME/.hasplm/`

       .. table::

             +-------------------------------------------------------------------------------------------+
             |IPアドレスが192.168.00.000の場合                                                           |
             +===========================================================================================+
             || serveraddr = 192.168.00.000                                                              |
             +-------------------------------------------------------------------------------------------+ 


.. _licenseupdate:

ライセンスの更新
=========================
support.nano@advancesoft.jp :sup:`*` にライセンスの更新をリクエストしてください。

.. role:: smallnote
   :class: small-note

:smallnote:`* このメールアドレスへの特定電子メール（広告・宣伝メール）の送信を拒否いたします。`

ライセンス登録後、noreply\@sentinelcloud.comから新しいEntitlement Certificateをメールでお送りしますので、記載されているProduct Keyを用いてライセンスの更新を行ってください。

基本的な操作方法は\ :ref:`licenseset`\ と同様です。ただし、以下の点に注意してください。

.. warning::

      C2VファイルおよびV2CPファイルは必ず新たに生成したものを使用してください。過去の設定・更新時に生成したものを誤って使用しないようにご注意ください。

.. warning::

      ACCのSentinel Keys画面からC2Vファイルをダウンロードする際は、必ず、更新を適用するキーに表示されているC2Vボタンをクリックしてダウンロードを行ってください。
  
.. note::

      初めてSentinelライセンスの設定を行う場合に必要なC2Vファイルのファイル名は :file:`fingerprint_32462.c2v` ですが、更新の際に必要なC2Vファイルのファイル名は :file:`(KeyID)_(timestamp).c2v` となります。
