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

また、永続的に追加するには、システムのプロパティから環境変数 :envvar:`Path` を編集してください。

Quantum ESPRESSOでDFT計算(SCF計算)を実行するには、 :file:`pw.exe` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`PW.inp` の実行例

 pw.exe -in PW.inp 1> PW.out 2> PW.err

.. code-block:: console
 :caption: 並列実行

 mpiexec.exe -n 4 pw.exe -in PW.inp 1> PW.out 2> PW.err

LAMMPSで分子動力学計算を実行するには、 :file:`lammps.exe` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`lammps.in` の実行例

 lammps.exe < lammps.in 1> lammps.out 2> lammps.err

.. code-block:: console
 :caption: 並列実行

 mpiexec.exe -n 4 lammps.exe < lammps.in 1> lammps.out 2> lammps.err

.. _tooll:

Linux版
================================

NanoLabo Toolインストール先の :file:`exec.LINUX` にQuantum ESPRESSO( :file:`qe_parallel` )、LAMMPS( :file:`lammps_parallel` )、MPIの実行ファイルがあります。

実行時には :file:`mpi/lib` にある動的ライブラリが必要ですので、環境変数 :envvar:`LD_LIBRARY_PATH` に設定するため、以下を実行してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export LD_LIBRARY_PATH=/opt/AdvanceSoft/NeuralMD/exec.LINUX/mpi/lib:$LD_LIBRARY_PATH

また、実行ファイルのパスを環境変数 :envvar:`PATH` に設定していただくと便利です。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export PATH=/opt/AdvanceSoft/NeuralMD/exec.LINUX/qe_parallel:/opt/AdvanceSoft/NeuralMD/exec.LINUX/lammps_parallel:/opt/AdvanceSoft/NeuralMD/exec.LINUX/mpi/bin:$PATH

Quantum ESPRESSOでDFT計算(SCF計算)を実行するには、 :file:`pw.x` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`PW.inp` の実行例

 pw.x -in PW.inp 1> PW.out 2> PW.err

.. code-block:: console
 :caption: 並列実行例

 mpirun -n 4 pw.exe -in PW.inp 1> PW.out 2> PW.err

LAMMPSで分子動力学計算を実行するには、 :file:`lammps` に入力ファイルを渡して実行します。

.. code-block:: console
 :caption: 入力ファイル :file:`lammps.in` の実行例

 lammps < lammps.in 1> lammps.out 2> lammps.err

.. code-block:: console
 :caption: 並列実行例

 mpirun -n 4 lammps.exe < lammps.in 1> lammps.out 2> lammps.err
