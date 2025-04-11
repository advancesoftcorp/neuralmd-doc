.. _windows:

==============================
インストール手順 (Windows)
==============================

.. _preparew:

インストールの準備
==============================

本ソフトウェアのインストールには、インストーラーを使用します。インストーラーは本体の「Advance/NeuralMD」に加え、弊社で改修した計算エンジン（Quantum ESPRESSO・LAMMPS）を含む「Advance/NanoLabo Tool」が用意されています。それぞれ以下のリンクからダウンロードしてください。

 `Advance/NeuralMD (ver.1.9.2) <https://www.apps.advancesoft.jp/nanolabo/install_neuralmd_windows_v1.9.2.exe>`_

 `Advance/NanoLabo Tool (ver.3.0) <https://www.apps.advancesoft.jp/nanolabo/install_nanolabo_tool_windows_v3.0.exe>`_

.. _installerw:

インストール
=============================

まず、Advance/NeuralMDのインストーラーを実行します。言語の選択画面が出た場合は、インストール中に使用する言語を選択してください（ソフトウェア本体で使用する言語ではありません）。

画面の指示に従い、インストールの設定を行ってください。設定が終わると要約の画面が表示されます。

.. image:: /img/install_summary.png

最後の画面で完了をクリックすると、Advance/NeuralMDのインストールが終了します。

続けて、Advance/NanoLabo Toolのインストーラーを実行します。

画面の指示に従い、インストールの設定を行ってください。

.. image:: /img/install_tool.png

インストール後、最後の画面で完了をクリックすると、Advance/NanoLabo Toolのインストールが終了します。

Advance/NanoLabo Toolに同梱された計算エンジン（Quantum ESPRESSO・LAMMPS）の使用方法については、\ :doc:`/usage/tool`\ を参照してください。

.. _licensew:

ライセンスの設定
=============================

:ref:`licensesetupdate`\ の手順に従ってライセンスの設定を行ってください。

.. _upgradew:

更新・アップグレード
=============================

- トライアル版から製品版にアップグレードされる場合、新たにインストールを行う必要はありません。\ :ref:`licenseupdate`\ を参考にしてライセンスのみを更新してください。

- 新しいバージョンにアップデートされる場合、上書きインストールを行うことも可能ではありますが、あらかじめ以前のバージョンをアンインストールするか、インストール先を変更していただくことをお勧めします。

- メジャーバージョンが新しいNeuralMDにアップデートする場合は、ライセンスの更新が必要です。\ :ref:`licenseupdate`\ を参考にしてライセンスを更新してください。

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

   アンインストールの際に、インストールログファイルが残る場合があります。また、FlexNetライセンスをご利用の場合は、ライセンスファイルは削除されずに残ります。その際はお手数ですが手動で削除してください。
