.. _slhmc:

==============================================================
力場自動生成（自己学習ハイブリッドモンテカルロ法）の使用方法
==============================================================

.. _slhmc_flow:

使用の流れ
================

1. :ref:`初期構造の作成 <slhmc_step1>`

 初期構造1つを、Advance/NanoLaboなどを利用してユーザーが作成します。

2. :ref:`力場の自動生成 <slhmc_step2>`

 初期構造を元に、教師データの生成・力場の学習が自動的に進められます。

3. :ref:`LAMMPSによる分子動力学計算 <slhmc_step3>`

 作成した力場ファイルを用いて、分子動力学計算を実施します。
 計算には、当社にて Neural Network Potential の機能を追加したLAMMPSを使用します。

.. _slhmc_steps:

各手順の説明
======================

.. _slhmc_step1:

手順1　初期構造の作成
----------------------------------

教師データとして使用したいサンプルの原子構造を用意し、それを元に、Quantum ESPRESSOのSCF計算用の入力ファイルを作成します。
当社製品のAdvance/NanoLaboを使っていただくと、cif等の原子構造ファイルから簡単に入力ファイルを生成できます。

ファイル中の擬ポテンシャルのパスは、手順2で実際に計算を行うときのものを指定してください。例えば計算サーバーで実行する場合には、計算サーバー内にある擬ポテンシャルのディレクトリを指すようにします。

入力ファイルが用意できたら、必要な情報を出力させるために以下の設定を加えてください。

.. code-block:: none

 &CONTROL
    tprnfor = .true.
    tsannp  = .true.
    disk_io = 'none'
 /
 &SYSTEM
    nosym   = .true.
 /

作成した入力ファイルは\ :file:`espresso.in`\ というファイル名で保存してください。

一から作成せずに、 ``slhmc --template`` で出力される\ :file:`espresso.in`\ を編集して作成するという手順でも大丈夫です。

.. _slhmc_step2:

手順2　力場の自動生成
----------------------------------

NanoLabo Tool同梱の計算エンジンを使用される場合は、ライブラリ検索パス等の設定が必要になります。以下を参照し、設定して下さい。

.. toctree::
   :maxdepth: 2

   usage/tool.rst

力場の自動生成は、実行ファイル\ :file:`slhmc`\ からQuantum ESPRESSO、LAMMPS、NeuralMDを順次呼び出しながら進められます。そこで、まずはこれらが実行できるように設定を行います。

.. code-block:: console

 slhmc --template

を実行することで、必要な設定ファイルのテンプレートが出力されます。

:file:`slhmc.prop`

 自己学習ハイブリッドモンテカルロ法の設定ファイルです。以下を参照して編集してください。

 .. toctree::
    :maxdepth: 1

    slhmc/prop.rst

:file:`espresso.sh`\ , :file:`lammps.sh`\ , :file:`sannp.sh`\ （Windowsの場合は.bat）

 Quantum ESPRESSO、LAMMPS、NeuralMD実行用のファイルです。\ :file:`slhmc`\ からこれらのファイルが実行されます。必要に応じ、実行に必要な環境変数の設定（パス :envvar:`PATH`\ , :envvar:`LD_LIBRARY_PATH`\ 、ライセンス :envvar:`ADVANCED_LICENSE_FILE`\ ）や、並列実行の設定（ :file:`mpirun` コマンド、OpenMP並列数 :envvar:`OMP_NUM_THREADS`\ ）等を追記してください。ファイル内に書かれている入出力ファイル名は固定ですので、変更しないでください。

 .. code-block:: none
  :caption: :file:`espresso.sh`\ でMPI並列を行う場合の記述例

  OMP_NUM_THREADS=1
  mpirun -n 4 pw.x -in espresso.in 1> espresso.out 2> espresso.err

 .. note:: NeuralMD Windows版はMPI並列実行には非対応です。OpenMP並列は使用可能です。

:file:`sannp.prop`

 NeuralMDの設定ファイルです。必要に応じ、\ :doc:`usage/prop`\ を参照して編集してください。

 電荷の計算を行う場合は、\ :file:`sannp.prop`\ で ``withCharge 1`` を設定し、\ :file:`sannp.sh`\ で :option:`sannp --train-charge` の行のコメントアウトを外してください。

 |Delta|\ -NNPを使う場合は、\ :file:`sannp.prop`\ で ``withClassical 1`` を設定し、\ :file:`sannp.sh`\ で :option:`sannp --classical` の行のコメントアウトを外してください。

準備が終わったら、手順1で作成した\ :file:`espresso.in`\ も同じフォルダに配置し、

.. code-block:: console

 slhmc --calc

を実行することで、力場の作成が始まります。

.. note::

 slhmc自体の並列実行はできません。各sh(bat)ファイル内で並列実行を行うように設定を行ってください。

ジョブ管理システムをお使いの場合は、 ``slhmc --calc`` を実行するようなジョブスクリプトを作成し、ジョブ投入してください。この場合も、slhmc自体は並列実行せずに、各shファイル内で並列実行するように設定を行ってください。

.. hint::

 デフォルトでは最初に第一原理計算分子動力学計算を行ってそれを元に初期ニューラルネットワーク力場を作りますが、代わりに既に作成済みの力場を初期力場として使うこともできます。 :file:`slhmc.prop` で ``initialTrain 0`` を設定し、力場ファイル :file:`ffield.sannp` を同じフォルダに配置してから、slhmcを実行します。

.. hint::

 GPUを使って計算を実行するには、\ :file:`sannp.sh`\ で\ :file:`sannp`\ を\ :file:`sannp_gpu`\ に、\ :file:`lammps.sh`\ で\ :file:`lammps`\ を\ :file:`lammps_gpu`\ にそれぞれ書き換えます。また、GPU用設定ファイル\ :ref:`sannp.mpi2gpu <usage_gpu_mpi>`\ 、\ :ref:`gpu.conf <toollammpsgpuconf>`\ を同じフォルダに配置すると、それぞれ実行時に設定内容が適用されます。

``slhmc --stop`` を実行すると、力場の作成を中断します。\ :file:`slhmc.prop`\ で ``restart 1`` と設定し、 ``slhmc --calc`` を再度実行することで、中断したところから力場の作成を再開します。

実行中には、力場更新のタイミングで力場ファイル\ :file:`ffield.sannp`\ が随時出力されます。また、構造の履歴が\ :file:`slhmc.xyz`\ として出力されます。

各計算エンジンの入出力ファイルは、サブフォルダ\ :file:`slhmc_dat`\ に保存されます。

もしエラー等で計算が進まない場合は、\ :file:`slhmc.CRASH`\ に当該の計算エンジンの入出力ファイルの内容が保存されますので、確認してください。

.. |Delta| raw:: html

 &Delta;

.. _slhmc_step3:

手順3　LAMMPSによる分子動力学計算
-------------------------------------------------

出力された力場ファイル\ :file:`ffield.sannp`\ を使って、LAMMPSによる分子動力学計算を実行します。基本の使用方法のページにある\ :ref:`usage_step5`\ を参照してください。

.. hint::

 複数の異なる初期構造を使って自己学習ハイブリッドモンテカルロ法で力場を作りたい場合、まずはそれぞれの初期構造に対して手順2までを実行します。その後、得られた教師データ\ :file:`sannp.train`\ を結合し、これを使って改めてニューラルネットワークの学習（基本の使用方法のページにある\ :ref:`usage_step4`\ ）を実行します。
