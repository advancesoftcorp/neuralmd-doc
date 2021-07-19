.. _usage:

================
使用方法
================

使用の流れ
================

1. :ref:`サンプル構造の作成 <usage_step1>`

 力場作成のためのサンプル構造を、Advance/NanoLaboなどを利用してユーザーが作成します。
 複数個のサンプル構造を同時に使用して、一つの力場を作成することが可能です。

2. :ref:`ランダム構造の生成 <usage_step2>`

 サンプル構造を基にして、原子座標をランダムに変位させた多数の構造を自動的に生成します。
 また、生成された全構造について、第一原理計算を一括して実行するスクリプトも同時に出力します。

3. :ref:`Quantum ESPRESSOによる第一原理計算 <usage_step3>`

 生成されたスクリプトを実行して、第一原理計算を実施します。
 計算には当社にて改修を施したQuantum ESPRESSOを使用して、原子毎のエネルギーを出力します。

4. :ref:`ニューラルネットワークの学習（最適化） <usage_step4>`

 Quantum ESPRESSOにて計算された原子毎のエネルギーを教師データとして、
 ニューラルネットワークを最適化します。最適化計算には、当社の提供するツールを使用します。
 計算完了後、LAMMPSにて利用可能な力場ファイルとして ニューラルネットワークの情報を出力します。

5. :ref:`LAMMPSによる分子動力学計算 <usage_step5>`

 作成した力場ファイルを用いて、分子動力学計算を実施します。
 計算には、当社にて Neural Network Potential の機能を追加したLAMMPSを使用します。

各手順の説明
======================

.. _usage_step1:

手順1　サンプル構造の作成
----------------------------------

教師データとして使用したいサンプルの原子構造を用意し、それを元に、Quantum ESPRESSOのSCF計算用の入力ファイルを作成します。
当社製品のAdvance/NanoLaboを使っていただくと、cif等の原子構造ファイルから簡単に入力ファイルを生成できます。

ファイル中の擬ポテンシャルのパスは、手順3で実際に計算を行うときのものを指定してください。例えば計算サーバーで実行する場合には、計算サーバー内にある擬ポテンシャルのディレクトリを指すようにします。

入力ファイルが用意できたら、必要な情報を出力させるために以下の設定を加えてください。

.. code-block:: none

 &CONTROL
    tprnfor = .true.
    tsannp = .true.
 /
 &SYSTEM
    nosym = .true.
 /

.. _usage_step2:

手順2　ランダム構造の生成
----------------------------------

作成した入力ファイルを基に、Advance/NeuralMDを実行して多数のランダム構造を生成します。実行ファイルはインストール先のbinフォルダーに入っている :file:`sannp` です。ファイル名が\ :file:`espresso.scf.in`\ の場合、

.. code-block:: console
 
 ./sannp --dft espresso.scf.in

として実行します。

対話形式で、生成する構造の数や変位の大きさなど、必要な情報を入力していきます。入力を行わずにそのままEnterを押すと、デフォルトの値が使われます。

入力が終わると、Quantum ESPRESSOの計算実行用シェルスクリプト\ :file:`dft_run.sh`\ （Windowsの場合はバッチファイル\ :file:`dft_run.bat`\ ）と、各構造の入力ファイルが生成されます。

| ├ dft_run.sh (.bat)
| └ dft_inp
|    ├ inp.1
|    ├ inp.2
|    ├ ...

.. _usage_step3:

手順3　Quantum ESPRESSOによる第一原理計算
----------------------------------------------

NanoLabo Tool同梱のQuantum ESPRESSOを使用される場合は、ライブラリ検索パス等の設定が必要になります。以下を参照し、設定して下さい。

.. toctree::
   :maxdepth: 2

   usage/tool.rst

ジョブ管理システムをお使いでない場合は、\ :file:`dft_run.sh`\ を直接実行します。

.. code-block:: console
 
 ./dft_run.sh

Windowsの場合は、\ :file:`dft_run.bat`\ を実行します。

.. code-block:: console

 dft_run.bat

ジョブ管理システムをお使いの場合には、必要に応じて\ :file:`dft_run.sh`\ を編集してください。例えばPBSの場合は#PBSディレクティブ、SLURM（FOCUSスパコン）の場合は#SBATCHディレクティブを追記します。その後、ジョブスクリプトとして\ :file:`dft_run.sh`\ を投入してください。

.. code-block:: console
 :caption: PBSの例

 qsub dft_run.sh

計算を実行すると、\ :file:`sannp.train`\ というファイルに教師データ（原子構造とそれに対応する原子毎のエネルギーや力、電荷）が保存されていきます。

.. hint::

 分子動力学計算を実行する系の原子の幾何構造を漏れなくかつ均一にサンプルするように、必要に応じて手順1～手順3を繰り返し、複数の\ :file:`sannp.train`\ ファイルを用意します。
 複数の\ :file:`sannp.train`\ は結合して使えます。

.. _usage_step4:

手順4　ニューラルネットワークの学習（最適化）
-------------------------------------------------

ニューラルネットワークの設定をファイル\ :file:`sannp.prop`\ 、Behler対称関数の設定をファイル\ :file:`sannp.behler`\ にそれぞれ用意します。デフォルトの設定を使う場合は、ファイルが無くてもかまいません。

.. toctree::
   :maxdepth: 2

   usage/prop
   usage/behler
   
電荷の計算を行わない場合(\ :file:`sannp.prop`\ で ``withCharge 0`` と設定)は、以下のコマンドを実行して、ニューラルネットワークの学習を行います。

.. code-block:: console
 
 ./sannp --train

Linuxの場合は、MPI並列での実行が可能です。NeuralMDインストーラーに同梱のMPI実行ファイル、またはNanoLabo Toolインストーラーに同梱のMPI実行ファイルをお使い下さい。\ :ref:`設定方法<tooll>`\ を参照して設定の上、実行してください。

.. code-block:: console
 :caption: MPI並列実行例(Linux)

 mpirun -n 4 sannp --train

実行中は残差RMS、経過時間等が出力されます。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ が出力されます。

学習を中断する場合は、 :option:`sannp --stop` を実行します。

収束せずに上限回数に達した場合や、上記コマンドで中断した場合は、\ :file:`sannp.prop`\ で ``restart 1`` と設定して再度実行することで、既に学習したニューラルネットワークの情報を読み込み、学習を再開できます。

また、電荷の計算を行う場合(\ :file:`sannp.prop`\ で ``withCharge 1`` と設定)は、上記に加えて電荷のニューラルネットワークの学習も行います。

.. code-block:: console
 
 ./sannp --train-charge

実行が正常に終わると、電荷のニューラルネットワークの情報を含むファイル\ :file:`sannp.data_q`\ が出力されます。

ニューラルネットワークの学習が終わったら、

.. code-block:: console
 
 ./sannp --export

を実行することで、LAMMPSで使用する力場定義ファイル\ :file:`ffield.sannp`\ を出力します。

.. _usage_step5:

手順5　LAMMPSによる分子動力学計算
-------------------------------------------------

NanoLabo Tool同梱のLAMMPSを使用される場合は、ライブラリ検索パス等の設定が必要になります。以下を参照し、設定して下さい。

.. toctree::
   :maxdepth: 2

   usage/tool.rst

LAMMPSの入力ファイル中で、以下の\ ``pair_style``\ が使えます。

.. describe:: pair_style nnp
 
 ニューラルネットワーク力場

 .. code-block:: none
  :caption: 書式

  pair_style nnp
  pair_coeff * * <ffield.sannp> <元素名1 元素名2 ...>

.. describe:: pair_style nnp/coul/cut

 ニューラルネットワーク力場 + クーロン相互作用（実空間）

 .. code-block:: none
  :caption: 書式

  pair_style nnp/coul/cut <rcut>
  pair_coeff * * <ffield.sannp> <元素名1 元素名2 ...>

.. describe:: pair_style nnp/coul/long

 ニューラルネットワーク力場 + クーロン相互作用（実空間 + k空間）

 .. code-block:: none
  :caption: 書式

  pair_style nnp/coul/long <rcut>
  pair_coeff * * <ffield.sannp> <元素名1 元素名2 ...>
  kspace_modify gewald <rinv>

パラメータ

 .. table::
  :widths: auto

  +------------------------------------+-------------------------------------------------------------------------------------------------+
  | ffield.sannp                       | 手順4で出力した力場ファイル名                                                                   |
  +------------------------------------+-------------------------------------------------------------------------------------------------+
  | 元素名                             | LAMMPSのatom type毎に、対応する教師データ中の元素名（=Quantum ESPRESSO計算時の元素名）を列挙    |
  +------------------------------------+-------------------------------------------------------------------------------------------------+
  | rcut                               || 実空間（短距離）クーロン相互作用のカットオフ半径                                               |
  |                                    || (\ ``nnp/coul/cut``\ では必ず指定、\ ``nnp/coul/long``\ では省略可)  (\ |rarr|\ `coul/cut`_)   |
  +------------------------------------+-------------------------------------------------------------------------------------------------+
  | rinv                               || 電荷のEwald和を計算する際のパラメータ（1/距離単位）                                            |
  |                                    || (\ ``nnp/coul/long``\ では必ず指定)  (\ |rarr|\ `kspace_modify`_)                              |
  +------------------------------------+-------------------------------------------------------------------------------------------------+
  
いずれかの\ ``pair_style``\ を設定した上でLAMMPSを実行すると、ニューラルネットワーク力場を使った分子動力学計算が行われます。

.. _`coul/cut`: https://lammps.sandia.gov/doc/pair_coul.html

.. _`kspace_modify`: https://lammps.sandia.gov/doc/kspace_modify.html

.. |rarr| raw:: html

   &rarr;

.. _additional_features:

追加機能の使用手順
======================

.. _hybridnnp_usage:

|Delta|\ -NNP
-------------------------------------------------

ニューラルネットワークの学習（手順4）の前に、古典力場のパラメータの最適化を行います。

.. code-block:: console
 
 ./sannp --classical

を実行することで、古典力場のパラメータを含むファイル\ :file:`sannp.class`\ が出力されます。

その後、\ :file:`sannp.prop`\ で ``withClassical 1`` と設定して、手順4以降を実行します。

.. _metropolis_usage:

メトロポリス法による構造生成
-------------------------------------------------

ニューラルネットワークの学習（手順4）が終わった後に実行します。

メトロポリス法の設定をファイル\ :file:`sannp.metro`\ に用意します。デフォルトの設定を使う場合は、ファイルが無くてもかまいません。

.. toctree::
   :maxdepth: 2

   usage/metro

.. code-block:: console
 
 ./sannp --metro

を実行すると、モンテカルロ計算が行われ、構造が生成されます。生成された構造はQuantum ESPRESSOの入力ファイルの形で\ :file:`dft_geom`\ フォルダに出力されます。

.. hint::

 モンテカルロ計算の過程はmovieフォルダにxyz形式で保存されます。
 
 xyzファイルをAdvance/NanoLaboの画面にドラッグ&ドロップすることで動画として可視化ができます。

続けて

.. code-block:: console
 
 ./sannp --dft

を実行すると、\ :file:`dft_geom`\ 内の計算を実行するためのシェルスクリプト\ :file:`dft_run.sh`\ （Windowsの場合はバッチファイル\ :file:`dft_run.bat`\ ）が生成されます。これを使って、再度手順3から実行し、教師データを追加します。

.. _usage_options:

実行オプションリスト
======================

.. program:: sannp

.. option:: --dft <file>, --qe <file>, --quantum-espresso <file>

 Quantum ESPRESSOの1つの入力ファイルから、教師データを作成するために多数のランダムな構造の入力ファイル\ :file:`*.inp`\ と実行用スクリプト\ :file:`dft_run.sh`\ を生成します。

 fileを指定しない場合、対話形式で入力を求められます。

.. option:: --omp <n>, --cpu <n>

 OpenMP並列（スレッド並列）の並列数を指定します。

.. option:: --train, --train-energy

 教師データ :file:`sannp.train`\ を元に、エネルギー及び力の計算を行うためのニューラルネットワークの学習を行います。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ が出力されます。

.. option:: --train-charge

 教師データ :file:`sannp.train`\ を元に、電荷の計算を行うためのニューラルネットワークの学習を行います。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data_q`\ が出力されます。

.. option:: --temp, --template

 設定ファイル\ :file:`sannp.prop`\ のテンプレートを出力します。既存のファイルは上書きされます。

.. option:: --behler, --behler-temp

 Behler対称関数の設定ファイル\ :file:`sannp.behler`\ のテンプレートを出力します。既存のファイルは上書きされます。

.. option:: --stop, --stop-training

 実行中の学習を中断し、学習途中のニューラルネットワークの情報を\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ または\ :file:`sannp.data_q`\ に出力します。

.. option:: --test

 学習したニューラルネットワークを使ってテスト用データの計算を行います。テスト用データのファイル\ :file:`sannp.test`\ は :option:`sannp --dft` を実行し、"test"を選ぶことで作成できます。結果として全エネルギー\ :file:`sannp.etot`\ 、原子毎のエネルギー\ :file:`sannp.eatom`\ 、原子に働く力\ :file:`sannp.force`\ 、電荷\ :file:`sannp.charge`\ がファイルに出力されます。

.. option:: --force, --force-test

 学習したニューラルネットワークを使ってテスト用データの計算を行い、結果として原子に働く力を表示します。テスト用データのファイル\ :file:`sannp.test`\ は :option:`sannp --dft` を実行し、"test"を選ぶことで作成できます。

.. option:: --export, --force-field, --lammps

 学習したニューラルネットワークから、LAMMPSで利用可能な力場ファイル\ :file:`ffield.sannp`\ を出力します。

.. option:: --metro, --monte-carlo, --mc

 学習後に実行すると、メトロポリス法を使ったモンテカルロ計算により構造を生成します。MPI並列実行時はレプリカ並列となります。実行後、生成された構造が\ :file:`dft_geom`\ フォルダに、各レプリカに対するモンテカルロ計算の動画が\ :file:`movie`\ フォルダにそれぞれ保存されます。続けて :option:`sannp --dft` を実行することで、生成された構造から教師データを作成するための実行用スクリプト\ :file:`dft_run.sh`\ を生成します。

.. option:: --metro-temp, --monte-carlo-temp, --mc-temp

 メトロポリス法の設定ファイル\ :file:`sannp.metro`\ のテンプレートを出力します。既存のファイルは上書きされます。GEOMETRYには教師データ\ :file:`sannp.train`\ 中の最初の構造が使われます。 

.. option:: --classical

 教師データ :file:`sannp.train`\ を元に、\ |Delta|\ -NNPで使用する古典力場のパラメータを最適化します。実行が正常に終わると、パラメータを含むファイル\ :file:`sannp.class`\ が出力されます。

.. _usage_double:

単精度版・倍精度版
======================

インストール先のbinフォルダーには通常の :file:`sannp` （単精度版）に加え、 :file:`sannp_d` （倍精度版）が入っています。もし倍精度版を使用されたいという場合は、説明中の :file:`sannp` を :file:`sannp_d` に読み替えて実行してください。

.. |Delta| raw:: html

 &Delta;
