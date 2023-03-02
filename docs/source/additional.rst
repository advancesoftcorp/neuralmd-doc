.. _additional_features:

====================
追加機能の使用手順
====================

.. _deltannp_usage:

LJ-like力場を用いた\ |Delta|\ -NNP
-------------------------------------------------

ニューラルネットワークの学習（手順4）の前に、古典力場のパラメータの最適化を行います。

.. code-block:: console

 sannp --classical

を実行することで、古典力場のパラメータを含むファイル\ :file:`sannp.class`\ が出力されます。

その後、\ :file:`sannp.prop`\ で ``withLJlike 1`` と設定して、手順4以降を実行します。

.. _deltannp_reax_usage:

ReaxFFを用いた\ |Delta|\ -NNP法
-------------------------------------------------

ニューラルネットワークの学習（手順4）で、\ :file:`sannp.prop`\ で ``withReaxFF 1`` と設定して実行します。

学習時、および分子動力学計算実行時（手順5）にパラメータ定義ファイル\ :file`ffield.reax`\ が必要です。NanoLabo Toolに同梱されているものをお使いいただけます。

.. hint:: ReaxFFのパラメータはあらかじめ用意されたものを使い、本製品での最適化は行いません。

.. _metropolis_usage:

メトロポリス法による構造生成
-------------------------------------------------

ニューラルネットワークの学習（手順4）が終わった後に実行します。

メトロポリス法の設定をファイル\ :file:`sannp.metro`\ に用意します。デフォルトの設定を使う場合は、ファイルが無くてもかまいません。

.. toctree::
   :maxdepth: 2

   usage/metro

.. code-block:: console

 sannp --metro

を実行すると、モンテカルロ計算が行われ、構造が生成されます。生成された構造はQuantum ESPRESSOの入力ファイルの形で\ :file:`dft_geom`\ フォルダに出力されます。

.. hint::

 モンテカルロ計算の過程はmovieフォルダにxyz形式で保存されます。

 xyzファイルをAdvance/NanoLaboの画面にドラッグ&ドロップすることで動画として可視化ができます。

続けて

.. code-block:: console

 sannp --dft

を実行すると、\ :file:`dft_geom`\ 内の計算を実行するためのシェルスクリプト\ :file:`dft_run.sh`\ （Windowsの場合はバッチファイル\ :file:`dft_run.bat`\ ）が生成されます。これを使って、再度手順3から実行し、教師データを追加します。

.. _insitu_usage:

In-situテスト
--------------------------

ニューラルネットワークの学習（手順4）を行う際に、同時にテストデータについても残差RMSを計算して表示する機能です。学習を進めながら、テストデータに対する性能を随時確認できます。また、教師データのRMSが小さくなっていてもテストデータのRMSが大きくなっている場合には、過学習を起こしていると判断できます。

まず、教師データとは別のテストデータを用意します。 :option:`sannp --dft` を実行し、"test"を選んで生成するか、または既存の教師データ\ :file:`sannp.train`\ を :option:`sannp --split` で分割します。テストデータ\ :file:`sannp.test`\ が用意できたら、\ :file:`sannp.prop`\ で ``insituTest 1`` と設定して、学習を実行します。

.. _asm_conv:

ASEのトラジェクトリファイルの変換
-----------------------------------

Atomic Simulation Environment(ASE)の\ `トラジェクトリ(.traj)ファイル <https://wiki.fysik.dtu.dk/ase/ase/io/trajectory.html>`_\ を教師データに変換し、ニューラルネットワークの学習に使うことができます。

インストール先の :file:`python` フォルダーにあるPythonスクリプト :file:`traj_to_train.py` を使い、

.. code-block:: console

 python traj_to_train.py filename.traj

と実行すると、教師データ :file:`sannp.train` が出力されます。ニューラルネットワークの学習（手順4）以降は通常の手順と同様です。

以下のオプションが使えます。

.. program:: python traj_to_train.py

.. option:: -output_file <file_name>

 出力ファイルを指定します。デフォルトは :file:`sannp.train` です。

.. option:: -mode {a,w,x}

 ファイル出力時の動作を指定します。 ``a`` : 追記、 ``w`` : 上書き、``x`` : 新規作成（ファイルが既に存在する場合はエラー）です。デフォルトは ``x`` です。

.. option:: -start <int>

 変換するデータの最初のインデックスです。デフォルトは0です。

.. option:: -step <int>

 変換するデータのステップです。デフォルトは1です。

.. |Delta| raw:: html

 &Delta;
