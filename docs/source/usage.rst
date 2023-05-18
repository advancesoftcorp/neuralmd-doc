.. _usage:

================
基本の使用方法
================

.. _usage_flow:

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

.. _usage_steps:

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

 sannp --dft espresso.scf.in

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

 sannp --train

Linuxの場合は、MPI並列での実行が可能です。NeuralMDインストーラーに同梱のMPI実行ファイル、またはNanoLabo Toolインストーラーに同梱のMPI実行ファイルをお使い下さい。\ :ref:`設定方法<tooll>`\ を参照して設定の上、実行してください。

.. code-block:: console
 :caption: MPI並列実行例(Linux)

 mpirun -n 4 sannp --train

.. note:: Windows版はMPI並列実行には非対応です。OpenMP並列は使用可能です。

実行中は残差RMS、経過時間等が出力されます。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ が出力されます。

学習を中断する場合は、 :option:`sannp --stop` を実行します。

収束せずに上限回数に達した場合や、上記コマンドで中断した場合は、\ :file:`sannp.prop`\ で ``restart 1`` と設定して再度実行することで、既に学習したニューラルネットワークの情報を読み込み、学習を再開できます。

また、電荷の計算を行う場合(\ :file:`sannp.prop`\ で ``withCharge 1`` と設定)は、上記に加えて電荷のニューラルネットワークの学習も行います。

.. code-block:: console

 sannp --train-charge

実行が正常に終わると、電荷のニューラルネットワークの情報を含むファイル\ :file:`sannp.data_q`\ が出力されます。

ニューラルネットワークの学習が終わったら、

.. code-block:: console

 sannp --export

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
  pair_coeff * * <ffield.sannp> <eatom zero|finite> <元素名1 元素名2 ...>

.. describe:: pair_style nnp/coul/cut

 ニューラルネットワーク力場 + クーロン相互作用（実空間）

 .. code-block:: none
  :caption: 書式

  pair_style nnp/coul/cut <rcut>
  pair_coeff * * <ffield.sannp> <eatom zero|finite> <元素名1 元素名2 ...>

.. describe:: pair_style nnp/coul/long

 ニューラルネットワーク力場 + クーロン相互作用（実空間 + k空間）

 .. code-block:: none
  :caption: 書式

  pair_style nnp/coul/long <rcut>
  pair_coeff * * <ffield.sannp> <eatom zero|finite> <元素名1 元素名2 ...>
  kspace_modify gewald <rinv>

パラメータ

 .. table::
  :widths: auto

  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+
  | ffield.sannp                       | 手順4で出力した力場ファイル名                                                                                             |
  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+
  | 元素名                             | LAMMPSのatom type毎に、対応する教師データ中の元素名（=Quantum ESPRESSO計算時の元素名）を列挙                              |
  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+
  | eatom                              || ニューラルネットワーク最終層のバイアス項（原子の内部エネルギーに相当する定数）を0に設定する機能（原子エネルギーの平準化）|
  |                                    || eatom zeroで有効、eatom finiteまたは省略（力場ファイル名に続けて元素名を指定）で無効                                     |
  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+
  | rcut                               || 実空間（短距離）クーロン相互作用のカットオフ半径                                                                         |
  |                                    || （\ ``nnp/coul/cut``\ では必ず指定、\ ``nnp/coul/long``\ では省略可）  (\ |rarr|\ `coul/cut`_)                           |
  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+
  | rinv                               || 電荷のEwald和を計算する際のパラメータ（1/距離単位）                                                                      |
  |                                    || （\ ``nnp/coul/long``\ では必ず指定）  (\ |rarr|\ `kspace_modify`_)                                                      |
  +------------------------------------+---------------------------------------------------------------------------------------------------------------------------+

いずれかの\ ``pair_style``\ を設定した上でLAMMPSを実行すると、ニューラルネットワーク力場を使った分子動力学計算が行われます。

.. _`coul/cut`: https://docs.lammps.org/pair_coul.html

.. _`kspace_modify`: https://docs.lammps.org/kspace_modify.html

.. |rarr| raw:: html

   &rarr;

.. _usage_options:

実行オプションリスト
======================

.. program:: sannp

.. option:: --dft <file>, --qe <file>, --quantum-espresso <file>

 Quantum ESPRESSOの1つの入力ファイルから、教師データを作成するために多数のランダムな構造の入力ファイル\ :file:`*.inp`\ と実行用スクリプト\ :file:`dft_run.sh`\ を生成します。

 fileを指定しない場合、対話形式で入力を求められます。

.. option:: --train, --train-energy

 教師データ\ :file:`sannp.train`\ を元に、エネルギー及び力の計算を行うためのニューラルネットワークの学習を行います。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ が出力されます。

.. option:: --train-charge

 教師データ\ :file:`sannp.train`\ を元に、電荷の計算を行うためのニューラルネットワークの学習を行います。実行が正常に終わると、ニューラルネットワークの情報を含むファイル\ :file:`sannp.data_q`\ が出力されます。

.. option:: --stop, --stop-training

 実行中の学習を中断し、学習途中のニューラルネットワークの情報を\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ または\ :file:`sannp.data_q`\ に出力します。

.. option:: --test, --test-without-bias

 学習したニューラルネットワークを使ってテスト用データの計算を行います。テスト用データのファイル\ :file:`sannp.test`\ は :option:`sannp --dft` を実行し、"test"を選ぶことで作成できます。結果として全エネルギー\ :file:`sannp.etot`\ 、原子毎のエネルギー\ :file:`sannp.eatom`\ 、原子に働く力\ :file:`sannp.force`\ 、電荷\ :file:`sannp.charge`\ がファイルに出力されます。

 MPI並列計算には非対応です。

 原子エネルギーは、最終層のバイアス項（原子の内部エネルギーに相当する定数）を0にして平準化した値が出力されます。

.. option:: --test-with-bias

 :option:`--test` と同様ですが、原子エネルギーの平準化を行わずに出力します。

.. option:: --temp, --template

 設定ファイル\ :file:`sannp.prop`\ のテンプレートを出力します。既存のファイルは上書きされます。

.. option:: --behler, --behler-temp

 Behler対称関数の設定ファイル\ :file:`sannp.behler`\ のテンプレートを出力します。既存のファイルは上書きされます。

.. option:: --force, --force-test

 学習したニューラルネットワークを使ってテスト用データの計算を行い、結果として原子に働く力を表示します。テスト用データのファイル\ :file:`sannp.test`\ は :option:`sannp --dft` を実行し、"test"を選ぶことで作成できます。

 MPI並列計算には非対応です。

.. option:: --export, --force-field, --lammps

 学習したニューラルネットワークから、LAMMPSで利用可能な力場ファイル\ :file:`ffield.sannp`\ を出力します。

.. option:: --split <r>, --split-train <r>

 教師データ\ :file:`sannp.train`\ から割合\ :math:`r (0 \le r \le 1)`\ を指定してデータを抽出し、テストデータ\ :file:`sannp.test`\ を作ります。同時に、残り\ :math:`(1 - r)`\ のデータを新たな教師データ\ :file:`sannp.train`\ として出力します。

.. option:: --metro, --monte-carlo, --mc

 学習後に実行すると、メトロポリス法を使ったモンテカルロ計算により構造を生成します。MPI並列実行時はレプリカ並列となります。実行後、生成された構造が\ :file:`dft_geom`\ フォルダに、各レプリカに対するモンテカルロ計算の動画が\ :file:`movie`\ フォルダにそれぞれ保存されます。続けて :option:`sannp --dft` を実行することで、生成された構造から教師データを作成するための実行用スクリプト\ :file:`dft_run.sh`\ を生成します。

.. option:: --metro-temp, --monte-carlo-temp, --mc-temp

 メトロポリス法の設定ファイル\ :file:`sannp.metro`\ のテンプレートを出力します。既存のファイルは上書きされます。GEOMETRYには教師データ\ :file:`sannp.train`\ 中の最初の構造が使われます。

.. option:: --classical

 教師データ :file:`sannp.train`\ を元に、\ |Delta|\ -NNPで使用する古典力場のパラメータを最適化します。実行が正常に終わると、パラメータを含むファイル\ :file:`sannp.class`\ が出力されます。

.. _usage_options_sub:

他のオプションに追加して指定するオプション
------------------------------------------

.. program:: sannp

.. option:: --omp <n>, --cpu <n>

 OpenMP並列（スレッド並列）の並列数を指定します。

.. option:: --que

 ライセンスエラー（同時実行数上限）の場合に、実行できるようになるまで待機します。

 このオプションを指定しない場合、ライセンスエラー時にはすぐに終了します。

.. _usage_double:

単精度版・倍精度版
======================

インストール先のbinフォルダーには通常の :file:`sannp` （単精度版）に加え、 :file:`sannp_d` （倍精度版）が入っています。もし倍精度版を使用されたいという場合は、説明中の :file:`sannp` を :file:`sannp_d` に読み替えて実行してください。

.. _usage_gpu:

GPU版
======================

グラフィックカード(GPU)が搭載されているマシンの場合、GPUで処理を行うことで計算を高速化することができます。インストール先のbinフォルダーに入っている :file:`sannp_gpu` （単精度版）、 :file:`sannp_gpu_d` （倍精度版）がGPU版の実行ファイルです。説明中の :file:`sannp` をこれらのファイルに読み替えて実行してください。

設定ファイル\ :file:`sannp.prop`\ にはGPU版特有の設定項目もありますが、特に変更を加えなくても非GPU版の入力ファイルのままで問題なく実行できます。

.. note::

 - GPUドライバを事前にインストールしておく必要があります。CUDA 11.4.4を使用しており、これに対応するドライババージョン470.82.01以上が必要です。
 - 元素数が5以上の場合は、重み付き対称関数を使う（\ :file:`sannp.prop`\ で\ ``elemWeight 1``\ を設定する）必要があります。

.. _usage_gpu_mpi:

MPI並列との併用
----------------------

GPU版をMPI並列で実行すると、GPUをより効率的に使うことができます。各プロセスが独立してGPUとやり取りを行い、例えばあるプロセスがGPUで計算をしている間に、他のプロセスがGPUメモリへのデータ転送を行う、という風に並行して複数のタスクを実行させる仕組みです。

また、複数のGPUが搭載されているマシンの場合、使用するGPUを指定することができます。\ :file:`sannp.mpi2gpu`\ というファイルを作成し、各行にプロセスをどのデバイスIDのGPUに割り当てるかを書きます。例えば、グラフィックカードが2つ搭載されているマシンで、MPI8並列で実行し、4プロセスをデバイスID0のGPU、4プロセスをデバイスID1のGPUに割り当てる場合は次のようにします。

.. code-block:: none
 :caption: sannp.mpi2gpu

 0
 0
 0
 0
 1
 1
 1
 1

ファイルの行数とMPI並列数（プロセス数）が一致するようにしてください。デフォルト（\ :file:`sannp.mpi2gpu`\ がない場合）では、全てのプロセスがデバイスID0のGPUに割り当てられます。

なお、各GPUに割り当てられたデバイスIDは

.. code-block:: console

 nvidia-smi -L

を実行して確認できます。

.. hint:: 1GPUデバイス当たりMPIプロセス数を2～4程度に設定すると効率的に計算を実行できます。

.. hint:: NanoLabo Toolに同梱の計算エンジンについては、Quantum ESPRESSOはGPU非対応、LAMMPSはGPU対応です。詳細は\ :ref:`toollammpsgpu`\ をご参照ください。

.. |Delta| raw:: html

 &Delta;
