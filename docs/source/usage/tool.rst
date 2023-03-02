.. _tool:

=====================================
NanoLabo Tool同梱計算エンジン
=====================================

.. _toolw:

Windows版
==================================

NanoLabo Toolインストール先の :file:`exec.WIN` にQuantum ESPRESSO( :file:`qe` )、LAMMPS、MPIの実行ファイルがあります。

実行ファイルのパスを環境変数 :envvar:`Path` に設定していただくと便利です。一時的に追加するには、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 set Path=C:\Program Files\AdvanceSoft\NanoLabo\exec.WIN\qe;C:\Program Files\AdvanceSoft\NanoLabo\exec.WIN\lammps;C:\Program Files\AdvanceSoft\NanoLabo\exec.WIN\mpi;%Path%

NanoLabo Toolインストール先の :file:`bin` にある :file:`NanoLabo.bat` を実行すると、 :envvar:`Path` が設定された状態でコマンドプロンプトが起動します。

また、永続的に追加するには、システムのプロパティから環境変数 :envvar:`Path` を編集してください。

Quantum ESPRESSOでDFT計算(SCF計算)を実行するには、 :file:`pw.exe` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`PW.inp` の実行例

 pw.exe -in PW.inp 1> PW.out 2> PW.err

.. code-block:: console
 :caption: 並列実行例

 mpiexec.exe -n 4 pw.exe -in PW.inp 1> PW.out 2> PW.err

LAMMPSで分子動力学計算を実行するには、 :file:`lammps.exe` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`lammps.in` の実行例

 lammps.exe < lammps.in 1> lammps.out 2> lammps.err

.. code-block:: console
 :caption: 並列実行例

 mpiexec.exe -n 4 lammps.exe < lammps.in 1> lammps.out 2> lammps.err

.. _tooll:

Linux版
================================

NanoLabo Toolインストール先の :file:`exec.LINUX` にQuantum ESPRESSO( :file:`qe_parallel` )、LAMMPS( :file:`lammps_parallel` )、MPIの実行ファイルがあります。

実行時には :file:`mpi/lib` にある動的ライブラリが必要ですので、環境変数 :envvar:`LD_LIBRARY_PATH` に設定するため、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export LD_LIBRARY_PATH=/opt/AdvanceSoft/NanoLabo/exec.LINUX/mpi/lib:$LD_LIBRARY_PATH

また、環境変数 :envvar:`PATH`、およびOpen MPIの環境変数 :envvar:`OPAL_PREFIX` の設定が必要ですので、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export PATH=/opt/AdvanceSoft/NanoLabo/exec.LINUX/mpi/bin:$PATH
 export OPAL_PREFIX=/opt/AdvanceSoft/NanoLabo/exec.LINUX/mpi

.. note::

 MPIの実行ファイル・ライブラリはNeuralMD本体のインストーラー、NanoLabo Toolインストーラーの両方に含まれています。

 前者はインストール先の :file:`mpi` 、後者はインストール先の :file:`exec.LINUX/mpi` に配置されます。

 内容は同じものですので、環境変数にはどちらか片方のみを設定していただければ大丈夫です。

計算エンジンの実行ファイルのパスも環境変数 :envvar:`PATH` に設定していただくと便利です。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export PATH=/opt/AdvanceSoft/NanoLabo/exec.LINUX/qe_parallel:/opt/AdvanceSoft/NanoLabo/exec.LINUX/lammps_parallel:$PATH

Quantum ESPRESSOでDFT計算(SCF計算)を実行するには、 :file:`pw.x` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`PW.inp` の実行例

 pw.x -in PW.inp 1> PW.out 2> PW.err

.. code-block:: console
 :caption: 並列実行例

 mpirun -n 4 pw.x -in PW.inp 1> PW.out 2> PW.err

LAMMPSで分子動力学計算を実行するには、 :file:`lammps` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`lammps.in` の実行例

 lammps < lammps.in 1> lammps.out 2> lammps.err

.. code-block:: console
 :caption: 並列実行例

 mpirun -n 4 lammps < lammps.in 1> lammps.out 2> lammps.err

.. _toollammpsgpu:

GPU版LAMMPS
================================

LAMMPSの実行ファイルとして :file:`lammps` の代わりに :file:`lammps_gpu` を使うことで、ニューラルネットワーク力場の計算がGPUを使って行われるようになります。

入力ファイルは非GPU版と同じ内容で問題なく実行できますが、設定ファイル\ :file:`gpu.conf`\ を作成することでGPU版特有の設定ができます。

.. note::

 - GPUドライバを事前にインストールしておく必要があります。CUDA 11.4.4を使用しており、これに対応するドライババージョン470.82.01以上が必要です。
 - 元素数が5以上の場合は、力場作成時に重み付き対称関数を使っている必要があります。

.. _toollammpsgpuconf:

gpu.confの書式
--------------------------------

threads、atomBlock、mpi2Deviceが各セクションの始まりを表し、次の行以降がセクションの内容になります。

各セクションは省略可能です。省略した場合、デフォルト値が使われます。

各セクションの前後には空行またはコメント行（行頭を!か#にする）を入れられます。

.. describe:: threads

 :デフォルト: 256

 CUDAのブロック当たりのスレッド数です。上限は1024（CUDAの仕様）です。32（ワープサイズ）の倍数を推奨します。

.. describe:: atomBlock

 :デフォルト: 4096

 対称関数をGPUで計算するときに、ここで指定した数の原子ごとにまとめて処理を行います。

.. describe:: mpi2Device

 :デフォルト: 全て0

 複数のGPUが搭載されているマシンの場合、使用するGPUをデバイスIDで指定します。MPI並列で使用する場合は、各行にプロセスをどのデバイスIDのGPUに割り当てるかを書きます。行数とMPI並列数（プロセス数）が一致するようにしてください。

 各GPUに割り当てられたデバイスIDは ``nvidia-smi -L`` を実行して確認できます。

.. code-block:: none
 :caption: gpu.confの例

 threads
 512
 atomBlock
 1024
 #グラフィックカードが2つ搭載されているマシンで、MPI8並列で実行し、
 #4プロセスをデバイスID0のGPU、4プロセスをデバイスID1のGPUに割り当てる場合
 mpi2Device
 0
 0
 0
 0
 1
 1
 1
 1

.. hint:: 1GPUデバイス当たりMPIプロセス数を2～4程度に設定すると効率的に計算を実行できます。
