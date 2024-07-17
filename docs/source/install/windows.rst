.. _windows:

==============================
インストール手順 (Windows)
==============================

.. _preparew:

インストールの準備
==============================

本ソフトウェアのインストールには、インストーラーを使用します。インストーラーは本体の「Advance/NeuralMD」に加え、弊社で改修した計算エンジン（Quantum ESPRESSO・LAMMPS）を含む「Advance/NanoLabo Tool」が用意されています。それぞれ以下のリンクからダウンロードしてください。

 `Advance/NeuralMD (ver.1.9.2) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=2166>`_

 `Advance/NanoLabo Tool (ver.3.0) <https://www.nanolabo.advancesoft.jp/?sdm_process_download=1&download_id=2793>`_

.. _installerw:

インストール
=============================

まず、Advance/NeuralMDのインストーラーを実行します。言語の選択画面が出た場合は、インストール中に使用する言語を選択してください（ソフトウェア本体で使用する言語ではありません）。

画面の指示に従い、インストールの設定を行ってください。設定が終わると要約の画面が表示されます。

.. image:: /img/install_summary.png

インストールが終わると、ライセンス登録案内が表示されます。ライセンス登録がお済みでない場合は、必要な項目をご記入ください。次へをクリックすると、ライセンス登録メールの内容が表示されますので、弊社宛に送信してください。

.. image:: /img/install_license.png

最後の画面で完了をクリックすると、Advance/NeuralMDのインストールが終了します。

続けて、Advance/NanoLabo Toolのインストーラーを実行します。

画面の指示に従い、インストールの設定を行ってください。

.. image:: /img/install_tool.png

インストール後、最後の画面で完了をクリックすると、Advance/NanoLabo Toolのインストールが終了します。

Advance/NanoLabo Toolに同梱された計算エンジン（Quantum ESPRESSO・LAMMPS）の使用方法については、\ :doc:`/usage/tool`\ を参照してください。

.. _licensew:

ライセンスの設定
=============================

ライセンス登録後、原則5営業日以内にライセンスファイル( :file:`neumd.lic` )をお送りします。インストール先の :file:`license` フォルダーにコピーしてください。

実行時には、環境変数 :envvar:`ADVANCED_LICENSE_FILE` にライセンスファイルのパスが設定されている必要があります。他の弊社製品をお使いの場合、既に環境変数の値が設定されている場合がありますが、その場合はセミコロン区切りでパスを追加してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 set ADVANCED_LICENSE_FILE=%ADVANCED_LICENSE_FILE%;C:\Program Files\AdvanceSoft\NeuralMD\license\neumd.lic

ニューラルネットワークの学習を行う機能（\ :option:`sannp --train`\ 、\ :option:`sannp --train-charge`\ 、\ :option:`sannp --classical`\ ）を使う場合に、有効なライセンスが必要となります。その他の機能、およびQuantum ESPRESSO・LAMMPSを実行する際には、ライセンスは不要です。slhmc（自己学習ハイブリッドモンテカルロ法の実行ファイル）は、それ自身の実行にはライセンスは不要ですが、sannpを呼び出してニューラルネットワークの学習を行うためにライセンスが必要です。

.. _upgradew:

更新・アップグレード
=============================

- トライアル版から製品版にアップグレードされる場合、新たにインストールを行う必要はありません。ライセンスファイルのみ置き換えてください。

- 新しいバージョンにアップデートされる場合、上書きインストールを行うことも可能ではありますが、あらかじめ以前のバージョンをアンインストールするか、インストール先を変更していただくことをお勧めします。

.. _uninstallw:

アンインストール
=============================

次のいずれかの方法でアンインストーラーを起動してください。

* スタートボタンを右クリックし、「アプリと機能」を開きます。リストの中にあるAdvance/NeuralMDをクリックし、アンインストールボタンをクリックします。
* スタートメニューから「Windows システム ツール」内の「コントロール パネル」を開きます。「プログラムのアンインストール」（アイコン表示の場合は「プログラムと機能」）を開き、リストの中にあるAdvance/NeuralMDをダブルクリックします。
* インストール先の :file:`_NeuralMD_installation` フォルダーにある :file:`Change NeuralMD Installation.exe` を起動します。

画面の指示に従い、アンインストールを行ってください。

Advance/NeuralMDのアンインストールが終わったら、同様にAdvance/NanoLabo Toolをアンインストールしてください。

.. note::

   アンインストールの際に、インストール先のライセンスファイルは削除されずに残ります。また、インストールログファイルが残る場合があります。その際はお手数ですが手動で削除してください。