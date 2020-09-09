.. _linux:

============================
インストール手順 (Linux)
============================

.. _preparel:

インストールの準備
==============================

本ソフトウェアのインストールには、インストーラーを使用します。インストーラーは本体の「Advance/NeuralMD」に加え、弊社で改修した計算エンジン（Quantum ESPRESSO・LAMMPS）を含む「Advance/NanoLabo Tool」が用意されています。それぞれ以下のリンクからダウンロードしてください。

 `Advance/NeuralMD (ver.1.0) <https://github.com/advancesoftcorp/neuralmd-doc/releases/download/v1.0/install_neuralmd_linux_v1.0.bin>`_

 `Advance/NanoLabo Tool (ver.1.5) <https://github.com/advancesoftcorp/nanolabo-doc/releases/download/v1.5/install_nanolabo_tool_linux_v1.5.bin>`_

.. _installerl:

インストール
=============================

端末（ターミナル）で、インストーラーのあるディレクトリに移動します。実行権限を付与したら、まずAdvance/NeuralMDのインストーラーを実行します。

.. code-block:: console

 $ chmod +x install_neural_linux.bin install_nanolabo_tool_linux.bin
 $ ./install_neuralmd_linux.bin

言語の選択画面が出た場合は、インストール中に使用する言語を選択してください（ソフトウェア本体で使用する言語ではありません）。

画面の指示に従い、インストールの設定を行ってください。

.. note::

 書き込み権限のない場所にはインストールできません。インストール先として書き込み権限のある場所（ :file:`/home/ユーザー名/AdvanceSoft/NeuralMD` など）を指定するか、書き込み権限のあるユーザーでインストーラーを実行してください。

設定が終わると要約の画面が表示されます。

.. image:: /img/install_summary_l.png

インストールが終わると、ライセンス登録案内が表示されます。ライセンス登録がお済みでない場合は、必要な項目をご記入ください。次へをクリックすると、ライセンス登録メールの内容が表示されますので、弊社宛に送信してください。

.. image:: /img/install_license_l.png

.. note::

 メール本文の「ホストＩＤ」が空欄になっている場合、Host ID取得時に必要なredhat-lsbライブラリがインストールされていない可能性があります。その場合、次のコマンドをroot権限で実行してインストールしてください。

 .. code-block:: console

  # yum -y install redhat-lsb-core

 インストーラーで一旦「戻る」をクリックし、再度「次へ」をクリックしていただくことで、Host IDの取得を再試行します。

最後の画面で完了をクリックすると、Advance/NeuralMDのインストールが終了します。

続けて、Advance/NanoLabo Toolのインストーラーを実行します。

.. code-block:: console

 $ ./install_nanolabo_tool_linux.bin

画面の指示に従い、インストールの設定を行ってください。

.. image:: /img/install_tool_l.png

インストール後、最後の画面で完了をクリックすると、Advance/NanoLabo Toolのインストールが終了します。

Advance/NanoLabo Toolに同梱された計算エンジン（Quantum ESPRESSO・LAMMPS）の使用方法については、\ :doc:`/usage/tool`\ を参照してください。

.. _launchl:

ライセンスの設定
=============================

ライセンス登録後、原則5営業日以内にライセンスファイル( :file:`nanolabo.lic` )をお送りします。インストール先の :file:`license` ディレクトリにコピーしてください。

本ソフトウェアを使用する際には、ライセンスサーバー（ライセンス認証用のプログラム）を起動しておく必要があります。ライセンスサーバーの実行ファイルはインストール先の :file:`license/lmgrd` です。端末（ターミナル）でインストール先のディレクトリに移動したら、以下のコマンド例のように起動します。

.. code-block:: console

 $ license/lmgrd -c license/neumd.lic -l license/lmgrd.log

ライセンスサーバーの状態を表示するには、インストール先の :file:`license/lmstat` を使用します。

.. code-block:: console

 $ license/lmstat -a -c license/neumd.lic

また、ライセンスサーバーを終了するには、インストール先の :file:`license/lmdown` を使用します。

.. code-block:: console

 $ license/lmdown -c license/neumd.lic

実行時には、環境変数 :envvar:`ADVANCED_LICENSE_FILE` にライセンスファイルのパスが設定されている必要があります。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export ADVANCED_LICENSE_FILE=/opt/AdvanceSoft/NeuralMD/license/neumd.lic

ニューラルネットワークの学習を行う機能（\ :option:`sannp --train`\ 、\ :option:`sannp --train-charge`\ ）を使う場合に、有効なライセンスが必要となります。その他の機能、およびQuantum ESPRESSO・LAMMPSを実行する際には、ライセンスは不要です。

また、本製品とは別の弊社製品をお使いの場合、ライセンスファイルを結合することで、1つのライセンスサーバーで複数製品のライセンス認証が可能です。\ :file:`.lic`\ ファイルを単純に結合した後、重複する行（ ``SERVER`` 、 ``VENDOR`` 、 ``USE_SERVER`` ） を削除して、1つのライセンスファイルを作ります。その後、そのファイルを使ってライセンスサーバーを起動してください。

.. table::
 :widths: 100,100,10,100

 +------------------------------------+---------------------------------------++-------------------+
 | ファイル1.lic                      | ファイル2.lic                         || 結合ファイル.lic  | 
 +====================================+=======================================++===================+
 || SERVER ...                        || SERVER ...                           ||| SERVER ...       |
 || VENDOR ...                        || VENDOR ...                           ||| VENDOR ...       |
 || USE_SERVER                        || USE_SERVER                           ||| USE_SERVER       |
 || |featgreen|                       || |featblue|                           ||| |featgreen|      |
 || |nbsp|                            || |nbsp|                               ||| |featblue|       |
 +------------------------------------+---------------------------------------++-------------------+

.. |featblue| raw:: html

   <font color="blue">FEATURE ...<br>...</font>

.. |featgreen| raw:: html

   <font color="green">FEATURE ...<br>...</font>

.. |nbsp| raw:: html

   &nbsp;<br>&nbsp;

.. _upgradel:

更新・アップグレード
=============================

- トライアル版から製品版にアップグレードされる場合、新たにインストールを行う必要はありません。ライセンスファイルのみ置き換えてください。

- 新しいバージョンにアップデートされる場合、上書きインストールを行うことも可能ではありますが、あらかじめ以前のバージョンをアンインストールするか、インストール先を変更していただくことをお勧めします。

.. _uninstalll:

アンインストール
=============================

端末（ターミナル）でインストール先の :file:`_NeuralMD_installation` ディレクトリにある :file:`Change NeuralMD Installation` を起動します。

.. code-block:: console

 $ AdvanceSoft/NeuralMD/_NeuralMD_installation/Change\ NeuralMD\ Installation

画面の指示に従い、アンインストールを行ってください。

Advance/NeuralMDのアンインストールが終わったら、同様にAdvance/NanoLabo Toolをアンインストールしてください。

.. note::

   アンインストールの際に、インストール先のライセンスファイルは削除されずに残ります。また、インストールログファイルが残る場合があります。その際はお手数ですが手動で削除してください。
