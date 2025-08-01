.. _linux:

============================
インストール手順 (Linux)
============================

.. _preparel:

インストールの準備
==============================

本ソフトウェアのインストールには、インストーラーを使用します。それぞれ以下のリンクからダウンロードしてください。

- 本体

 `Advance/NeuralMD (ver.2.0) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=3096>`_

- 弊社で改修した計算エンジン（Quantum ESPRESSO・LAMMPS）

 `Advance/NanoLabo Tool (ver.3.1) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=3087>`_

- 追加のGNN力場モデルファイル（FAIR-ChemまたはSevenNetを使う場合のみ）

 `Advance/NanoLabo GNNP (ver.3.1) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=3085>`_

.. _installerl:

インストール
=============================

端末（ターミナル）で、インストーラーのあるディレクトリに移動します。実行権限を付与したら、まずAdvance/NeuralMDのインストーラーを実行します。

.. code-block:: console

 $ chmod +x install_neural_linux.bin install_nanolabo_tool_linux.bin
 $ sudo ./install_neuralmd_linux.bin

.. note::

 ライセンスマネージャー(Sentinel-LDK-RTE)をインストールするためにroot権限での実行が必要です。

 Sentinel-LDK-RTEのインストールが不要（既にインストール済みなど）という場合は一般ユーザーでも実行できます。

.. note::

 インストーラーはコンソールモード（GUIを表示せず、CUIで操作が完結する）でも動作します。もしGUIが無い環境で、実行時にコンソールモードにならない場合は

 .. code-block:: console

   $ ./install_neuralmd_linux.bin -i console

 のように実行してください。

言語の選択画面が出た場合は、インストール中に使用する言語を選択してください（ソフトウェア本体で使用する言語ではありません）。

画面の指示に従い、インストールの設定を行ってください。

.. note::

 書き込み権限のない場所にはインストールできません。インストール先として書き込み権限のある場所（ :file:`/home/ユーザー名/AdvanceSoft/NeuralMD` など）を指定するか、書き込み権限のあるユーザーでインストーラーを実行してください。

- フローティングライセンスでクライアントとして使用する場合、Sentinel-LDK-RTEをインストールしますか？でいいえを選択してください（一般ユーザーで実行した場合は確認は表示されません）。

  ライセンスサーバーが異なるネットワークセグメントに存在する場合は、次の画面でライセンスサーバーのIPアドレスまたはホスト名を入力してください。ここで指定しない場合でも、後からライセンスサーバーのIPアドレスまたはホスト名を設定することは可能です。詳細は\ :ref:`floating`\ を参照してください。

  .. image:: /img/install_license_server_l.png

設定が終わると要約の画面が表示されます。

.. image:: /img/install_summary_l.png

最後の画面で完了をクリックすると、Advance/NeuralMDのインストールが終了します。

続けて、Advance/NanoLabo Toolのインストーラーを実行します。

.. code-block:: console

 $ ./install_nanolabo_tool_linux.bin

画面の指示に従い、インストールの設定を行ってください。

.. image:: /img/install_tool_l.png

インストール後、最後の画面で完了をクリックすると、Advance/NanoLabo Toolのインストールが終了します。

FAIR-ChemまたはSevenNetを使う場合は、Advance/NanoLabo GNNPも同様にインストールしてください。

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

:ref:`licensesetupdate`\ の手順に従ってライセンスの設定を行ってください。

.. _upgradel:

更新・アップグレード
=============================

- トライアル版から製品版にアップグレードされる場合、新たにインストールを行う必要はありません。\ :ref:`licenseupdate`\ を参考にしてライセンスのみを更新してください。

- 新しいバージョンにアップデートされる場合、上書きインストールを行うことも可能ではありますが、あらかじめ以前のバージョンをアンインストールするか、インストール先を変更していただくことをお勧めします。

- メジャーバージョンが新しいNeuralMDにアップデートする場合は、ライセンスの更新が必要です。\ :ref:`licenseupdate`\ を参考にしてライセンスを更新してください。

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

.. note::

   Sentinel-LDK-RTEを後からアンインストールするには、\ `Sentinel-LDK-RTEインストーラー <https://apps.advancesoft.jp/sentinel/Sentinel-LDK-RTE-for-AdvanceSoft-v10.13.1_Linux.tar.gz>`_\ に入っている :file:`dunst` を実行してください。
