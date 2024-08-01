.. _linux:

============================
インストール手順 (Linux)
============================

.. _preparel:

インストールの準備
==============================

本ソフトウェアのインストールには、インストーラーを使用します。インストーラーは本体の「Advance/NeuralMD」に加え、弊社で改修した計算エンジン（Quantum ESPRESSO・LAMMPS）を含む「Advance/NanoLabo Tool」が用意されています。それぞれ以下のリンクからダウンロードしてください。

 `Advance/NeuralMD (ver.1.9.2) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=2164>`_

 `Advance/NanoLabo Tool (ver.3.0) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=2795>`_

.. _installerl:

インストール
=============================

端末（ターミナル）で、インストーラーのあるディレクトリに移動します。実行権限を付与したら、まずAdvance/NeuralMDのインストーラーを実行します。

.. code-block:: console

 $ chmod +x install_neural_linux.bin install_nanolabo_tool_linux.bin
 $ ./install_neuralmd_linux.bin

.. note::

 インストーラーはコンソールモード（GUIを表示せず、CUIで操作が完結する）でも動作します。もしGUIが無い環境で、実行時にコンソールモードにならない場合は

 .. code-block:: console

   $ ./install_neuralmd_linux.bin -i console

 のように実行してください。

言語の選択画面が出た場合は、インストール中に使用する言語を選択してください（ソフトウェア本体で使用する言語ではありません）。

画面の指示に従い、インストールの設定を行ってください。

.. note::

 書き込み権限のない場所にはインストールできません。インストール先として書き込み権限のある場所（ :file:`/home/ユーザー名/AdvanceSoft/NeuralMD` など）を指定するか、書き込み権限のあるユーザーでインストーラーを実行してください。

設定が終わると要約の画面が表示されます。

.. image:: /img/install_summary_l.png

.. iniファイルの生成

最後の画面で完了をクリックすると、Advance/NeuralMDのインストールが終了します。

続けて、Advance/NanoLabo Toolのインストーラーを実行します。

.. code-block:: console

 $ ./install_nanolabo_tool_linux.bin

画面の指示に従い、インストールの設定を行ってください。

.. image:: /img/install_tool_l.png

インストール後、最後の画面で完了をクリックすると、Advance/NanoLabo Toolのインストールが終了します。

Advance/NanoLabo Toolに同梱された計算エンジン（Quantum ESPRESSO・LAMMPS）の使用方法については、\ :doc:`/usage/tool`\ を参照してください。

NeuralMDの実行時には :file:`mpi/lib` にある動的ライブラリが必要ですので、環境変数 :envvar:`LD_LIBRARY_PATH` に設定するため、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export LD_LIBRARY_PATH=/opt/AdvanceSoft/NeuralMD/mpi/lib:$LD_LIBRARY_PATH

また、環境変数 :envvar:`PATH` 、およびOpen MPIの環境変数 :envvar:`OPAL_PREFIX` の設定が必要ですので、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export PATH=/opt/AdvanceSoft/NeuralMD/mpi/bin:$PATH
 export OPAL_PREFIX=/opt/AdvanceSoft/NeuralMD/mpi

.. note::

 MPIの実行ファイル・ライブラリはNeuralMD本体のインストーラー、NanoLabo Toolインストーラーの両方に含まれています。

 前者はインストール先の :file:`mpi` 、後者はインストール先の :file:`exec.LINUX/mpi` に配置されます。

 内容は同じものですので、環境変数にはどちらか片方のみを設定していただければ大丈夫です。

NeuralMDの実行ファイルのパスも環境変数 :envvar:`PATH` に設定していただくと便利です。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export PATH=/opt/AdvanceSoft/NeuralMD/bin:$PATH

.. _licensel:

ライセンスの設定
=============================

.. note::

   ニューラルネットワークの学習を行う機能（\ :option:`sannp --train`\ 、\ :option:`sannp --train-charge`\ 、\ :option:`sannp --classical`\ ）を使う場合に、有効なライセンスが必要となります。その他の機能、およびQuantum ESPRESSO・LAMMPSを実行する際には、ライセンスは不要です。slhmc（自己学習ハイブリッドモンテカルロ法の実行ファイル）は、それ自身の実行にはライセンスは不要ですが、sannpを呼び出してニューラルネットワークの学習を行うためにライセンスが必要です。

ライセンス登録後、noreply\@sentinelcloud.comからEntitlement Certificateをメールでお送りします。Entitlement Certificateに記載されているProduct Key (PKID)を用いてライセンスの設定を行います。

.. _licenseaccc2vl:

C2Vファイルの生成
+++++++++++++++++

 NeuralMDをインストールしているマシンのウェブブラウザで、 Admin Control Center (ACC) (http://localhost:1947) にアクセスします。

.. note::
      
     ACCはオフラインのマシンからもアクセス可能です。

.. note::
      
    NeuralMDをインストールしているマシンでGUIアプリケーションが使用できない場合、同一ネットワーク上の別のマシンのウェブブラウザからACCにアクセスして、C2Vファイルの生成を行うことも可能です。
    その場合は、まず、ACCのアクセスの設定をします。
    その後、ウェブブラウザのアドレスの欄にhttp://<NeuralMDをインストールしているマシンのIPアドレス>:1947と入力します。

ACCのSentinel Keys画面のリストのうち、Vendorの欄に32462と記載されている行の、Fingerprintボタンをクリックして、C2Vファイル（拡張子:c2v）をダウンロードします。

.. image:: /img/ACCSentinelKeys.png

.. _licenseaccv2cpl:

V2CPファイルの生成
+++++++++++++++++++

次に、ウェブブラウザで\ `EMS <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスします。

.. note::
      
      NeuralMDをインストールしたマシンがオフラインの場合は、ダウンロードしたC2Vファイルをオンラインの別のマシンに移動したうえで\ `EMS <https://advancesoftcorporation.prod.sentinelcloud.com/customer/>`_\ にアクセスしてください。


ログイン方法としてPKIDを選択し、Entitlement Certificateに記載されているProduct Key (PKID)を入力してログインします。

.. image:: /img/EMSLogin.png

Products画面が開いたら、Activate Offlineボタンをクリックします。

.. image:: /img/EMSProducts.png

Activate Products画面が開いたら、Select Fileボタンをクリックして、先ほどダウンロードしたC2Vファイルを選択し、Complete Activationボタンをクリックします。

.. image:: /img/EMSActivateProductsFingerprint.png

アクティベーションに成功したら、Download Licenseをクリックして、V2CPファイル(拡張子:.v2cp)をダウンロードします。

.. image:: /img/EMSActivatedFingerprint.png

.. _licenseaccv2cpapplyl:

V2CPファイルの適用
+++++++++++++++++++

ACCの画面に戻り、左側のメニューからUpdate/Attach画面を開きます。Select Fileボタンから、ダウンロードしたV2CPファイルを選択し、Apply Fileボタンをクリックしてください。

.. note::
      
      NeuralMDをインストールしたマシンでGUIアプリケーションが使用できない場合、C2Vファイルの生成の場合と同様に、同一ネットワーク上の別のマシンのウェブブラウザからACCにアクセスして、V2CPファイルの適用を行います。

.. note::
      
      NeuralMDをインストールしたマシンがオフラインの場合は、ダウンロードしたV2CPファイルをオフラインのマシンに移動したうえでACCにアクセスしてください。

.. image:: /img/ACCApply.png

V2CPファイルのApplyに成功したら、ライセンスの設定は完了となります。

.. _floatingl:

フローティングライセンス
+++++++++++++++++++++++++

フローティングライセンスをご購入いただいた場合は、ライセンスの設定を行ったマシンと同一のネットワーク上にある別のマシン（Windows・Linux）でもNeuralMDを使うことができます。

- ライセンスサーバーとして使うマシン側

 NeuralMDをインストールして、ライセンスの設定を行ってください。マシンの起動時に毎回自動でライセンスマネージャが起動するため、一度ライセンスの設定を行って以降は特に必要な操作はありません。

- NeuralMDを使うクライアントマシン側
  
 NeuralMDをインストールして下さい。通常、NeuralMDをインストールするだけで計算は実行可能となりますが、ライセンスエラーが発生する場合は、次のいずれかの方法でライセンスサーバーのIPアドレスを設定する必要があります。
  
 - クライアントマシンでGUIアプリケーションが使用可能な場合、ACCを利用して設定を行います。クライアントマシンから\ `Admin Control Center (ACC) <http://localhost:1947>`_\ にアクセスし、画面左側のメニューからConfiguration画面を開いてください。次に、Access to Remote License Managersタブを開いて、Remote License Search ParametersにライセンスサーバーのIPアドレスを入力し、Submitをクリックしてください。
  
 - クライアントマシンでGUIアプリケーションが使用できない場合、hasplm.iniファイルを作成して設定を行います。/etc/hasplm/にhasplm.iniファイルを作成し、以下の例を参考にしてライセンスサーバーのIPアドレスを記述してください。

   .. table::
 
      +-------------------------------------------------------------------------------------------+
      |IPアドレスが192.168.00.000の場合　　　　　　　　　　　　　　　　　　                       |
      +===========================================================================================+
      || serveraddr = 192.168.00.000                                                              |
      +-------------------------------------------------------------------------------------------+ 

.. note::
              
  ライセンスサーバーのファイアウォールの設定で、TCP/UDP ポート 1947が開放されていない場合は、設定を変更して開放してください。（Windowsマシンの場合、インストール時に自動でこれらのポートは開放されるため、通常ではファイアウォールの設定は必要ありません。）

.. _upgradel:

更新・アップグレード
=============================

- トライアル版から製品版にアップグレードされる場合、新たにインストールを行う必要はありません。以下の\ :ref:`licenseupdatel`\ を参考にしてライセンスのみを更新してください。

- 新しいバージョンにアップデートされる場合、上書きインストールを行うことも可能ではありますが、あらかじめ以前のバージョンをアンインストールするか、インストール先を変更していただくことをお勧めします。

.. _licenseupdatel:

ライセンスの更新
+++++++++++++++++++++++++++++

基本的な操作方法はライセンスの設定と同様です。ただし、以下の点に注意してください。

- support.nano\@advancesoft.jpにライセンスの更新をリクエストしてください。ライセンス登録後、noreply\@sentinelcloud.comから新しいEntitlement Certificateをメールでお送りしますので、記載されているProduct Key (PKID)を用いてライセンスの更新を行ってください。

- 初めてSentinelライセンスの設定を行う場合に必要なC2Vファイルのファイル名はfingerprint_32462.c2vですが、更新の際に必要なC2Vファイルのファイル名は(KeyID)_(timestamp).c2vとなります。

- ACCのSentinel Keys画面からC2Vファイルをダウンロードする際は、必ず、更新を適用するキーのC2Vボタンをクリックしてダウンロードを行ってください。

- EMS上では、fingerprint_32462.c2vではなく、必ず、手前の手順でダウンロードしたC2Vファイル((KeyID)_(timestamp).c2v)を使用してください。

.. _uninstalll:

アンインストール
=============================

端末（ターミナル）でインストール先の :file:`_NeuralMD_installation` ディレクトリにある :file:`Change NeuralMD Installation` を起動します。

.. code-block:: console

 $ AdvanceSoft/NeuralMD/_NeuralMD_installation/Change\ NeuralMD\ Installation

画面の指示に従い、アンインストールを行ってください。

Advance/NeuralMDのアンインストールが終わったら、同様にAdvance/NanoLabo Toolをアンインストールしてください。

.. note::

   アンインストールの際に、インストールログファイルが残る場合があります。また、FlexNetライセンスをご利用の場合は、ライセンスファイルは削除されずに残ります。その際はお手数ですが手動で削除してください。