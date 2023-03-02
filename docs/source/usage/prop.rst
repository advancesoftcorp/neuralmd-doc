.. _prop:

===================
sannp.propの書式
===================

\ :file:`sannp.prop`\ のテンプレートは、 :option:`sannp --temp` で出力できます。

行頭が#または!の行はコメントとして扱われます。

.. describe:: restart

 :デフォルト: 0

 0の場合、最初から学習を実行します。1の場合、ニューラルネットワークの情報を\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ または\ :file:`sannp.data_q`\ から読み込み、学習を再開します。

.. describe:: insituTest

 :デフォルト: 0

 1の場合、学習中の各epochで、テストデータに対するRSMEを計算して出力するin-situテストを行います。0の場合、in-situテストを行いません。

.. describe:: withCharge

 :デフォルト: 0

 電荷の計算を行う(1)か行わない(0)かの指定です。

.. describe:: withHDNNP

 :デフォルト: 1

 系全体のエネルギーを教師データとするHDNNP法を使う(1)か、各原子に分割したエネルギーを教師データとするSANNP法を使う(0)かの指定です。

.. describe:: withLJlike

 :デフォルト: 0

 LJ-like力場とニューラルネットワーク力場を組み合わせた\ |Delta|\ -NNPを使う(1)か使わない(0)かの指定です。ReaxFFを用いた \ |Delta|\ -NNPとは併用できません。 ``withClassical 1`` でも ``withLJlike 1`` と同じ設定になります。

.. describe:: withReaxFF

 :デフォルト: 0

 ReaxFFとニューラルネットワーク力場を組み合わせた\ |Delta|\ -NNPを使う(1)か使わない(0)かの指定です。ReaxFFを使う場合、パラメータ定義ファイル\ :file:`ffield.reax`\ が必要です。LJ-like力場を用いた \ |Delta|\ -NNPとは併用できません。 ``withClassical 2`` でも ``withReaxFF 1`` と同じ設定になります。

.. describe:: rcutReaxFF

 :デフォルト: 5.0

 ReaxFFを用いた \ |Delta|\ -NNPで、ReaxFFのカットオフ半径(\ |angs|\ )を指定します。

.. describe:: rateReaxFF

 :デフォルト: 0.5

 ReaxFFを用いた \ |Delta|\ -NNPで、エネルギーと力を計算する際のReaxFFの寄与（混合率）を指定します。

.. describe:: directSF

 :デフォルト: -1

 対称関数の正規化をミニバッチ内で行う(1)か、サンプル全体で行う(0)かの指定です。負の値を指定した場合、Behler対称関数ならサンプル全体、Many-Body対称関数ならミニバッチ内になります。

.. describe:: maxForce

 :デフォルト: 10.0

 教師データに含まれる、力が大きすぎる外れ値を除外するための閾値(eV/\ |angs|\ )です。0以下の値を指定した場合、除外を行いません。

.. describe:: minEDev

 :デフォルト: 0.5

 原子のエネルギーの正規化に使う分散の下限値(eV)を指定します。

.. describe:: minQDev

 :デフォルト: 0.1

 原子の電荷の正規化に使う分散の下限値(e)を指定します。

.. describe:: symmFunc

 :デフォルト: chebyshev

 対称関数を指定します。behler、chebyshev、many-bodyが指定できます。

.. describe:: elemWeight

 :デフォルト: 1

 重み付き対称関数を使う(1)か、使わない(0)かの指定です。Behler対称関数、Chebyshev対称関数で使用可能です。

.. describe:: tanhCutoff

 :デフォルト: 0

 カットオフ関数\ :math:`f_c(R_{ij})`\ としてtanhを使った関数を使う(1)か、cosを使った関数を使う(0)かの指定です。

.. describe:: m2

 :デフォルト: 100

 Many-Body対称関数のパラメータ *M*:sub:`2` です。

.. describe:: m3

 :デフォルト: 10

 Many-Body対称関数のパラメータ *M*:sub:`3` です。

.. describe:: rinner

 :デフォルト: 0.0

 Many-Body対称関数のパラメータ *R*:sub:`inner` (\ |angs|\ )です。

.. describe:: router

 :デフォルト: 6.5

 Many-Body対称関数のパラメータ *R*:sub:`outer` (\ |angs|\ )です。

.. describe:: numRadius

 :デフォルト: 20

 Chebyshev対称関数の動径成分の数を指定します。

.. describe:: numAngle

 :デフォルト: 20

 Chebyshev対称関数の角度成分の数を指定します。

.. describe:: rcutRadius

 :デフォルト: 6.5

 Chebyshev対称関数の動径成分のカットオフ距離 *R*:sub:`c` (\ |angs|\ )を指定します。

.. describe:: rcutAngle

 :デフォルト: 6.5

 Chebyshev対称関数の角度成分のカットオフ距離 *R*:sub:`c` (\ |angs|\ )を指定します。

.. describe:: layers

 :デフォルト: 2

 ニューラルネットワークの隠れ層の層数です。

.. describe:: nodes

 :デフォルト: 30

 ニューラルネットワークのノード数です。

.. describe:: activ

 :デフォルト: twtanh

 ニューラルネットワークの活性化関数です。asis（使用しない）、sigmoid、tanh、twtanh(twisted tanh)、eLU、GELUが指定できます。

.. describe:: lbfgs

 :デフォルト: 32

 学習時の最適化アルゴリズムの指定です。0を指定すると、Adam法を使用します。1以上の値を指定すると、その値を履歴数とするL-BFGS法を使用します。

.. describe:: lineSearch

 :デフォルト: more-thuente

 L-BFGS法で使用する直線探索のアルゴリズムです。more-thuente、armijo、wolfe、strong-wolfeが指定できます。

.. describe:: lineSteps

 :デフォルト: 32

 L-BFGS法で使用する直線探索の試行回数の最大値です。

.. describe:: batchs

 :デフォルト: 0

 学習時のミニバッチサイズです。0以下の値を指定した場合、ミニバッチは使用せず、フルバッチ（サンプル全体）で学習を行います。

.. describe:: epochs

 :デフォルト: 5000

 学習時の繰り返し回数(epoch)の上限です。

.. describe:: epochsStore

 :デフォルト: 1000

 学習中にニューラルネットワークのデータをファイルに保存する間隔を設定します。0以下の値を設定すると終了時のみ保存します。

.. describe:: epochsOnlyE

 :デフォルト: 250

 学習時の繰り返し回数(epoch)がこの数字より小さいうちはエネルギーのみを使って学習を行います。それ以降はエネルギーと原子に働く力の両方を使って学習を行います。

.. describe:: epochsApproxF

 :デフォルト: 500

 エネルギーから原子に働く力の誤差を計算する際、繰り返し回数(epoch)がこの数字より小さいうちはDouble Backward法による近似的な微分法を使います。それ以降は厳密に微分を計算します。

.. describe:: sqrtLoss

 :デフォルト: 0

 損失関数のスケール係数を絶対値で設定する(1)か、2乗値で設定する(0)かの指定です。

.. describe:: renormLoss

 :デフォルト: 0

 損失関数の正規化を行う(1)か行わない(0)かの指定です。

.. describe:: approxForce

 :デフォルト: 0

 エネルギーから原子に働く力の誤差を計算する際、epochsApproxFを使って近似的な微分と厳密な微分を切り替える(0)か、常に近似的な微分法を使う(1)か設定します。

.. describe:: rmseEnergy

 :デフォルト: 0.01

 学習が収束したか判定するためのエネルギー残差(RMS)の閾値(eV/atom)です。

.. describe:: rmseForce

 :デフォルト: 0.10

 学習が収束したか判定するための力の残差(RMS)の閾値(eV/\ |angs|\ )です。

.. describe:: rmseCharge

 :デフォルト: 0.01

 学習が収束したか判定するための電荷の残差(RMS)の閾値(e)です。

.. describe:: coefEnergy

 :デフォルト: 1.00

 エネルギーの損失関数のスケール係数(1/eV または 1/eV\ :sup:`2`\ )です。

.. describe:: coefForce

 :デフォルト: 1.00

 力の損失関数のスケール係数(\ |angs|\ /eV または (\ |angs|\ /eV)\ :sup:`2`\ )です。

.. describe:: coefCharge

 :デフォルト: 1.00

 電荷の損失関数のスケール係数(1/e または 1/e\ :sup:`2`\ )です。

.. describe:: learnRate

 :デフォルト: 1.0e-4

 学習率の初期値です。

.. describe:: learnRateFinal

 :デフォルト: 1.0e-4

 学習率の下限値です。

.. describe:: learnRateDecay

 :デフォルト: 0.9999

 学習率の減衰率です。

.. describe:: adamBeta1

 :デフォルト: 0.9

 学習時のハイパーパラメータ（Adam法の\ |beta|\ :sub:`1`）です。

.. describe:: adamBeta2

 :デフォルト: 0.999

 学習時のハイパーパラメータ（Adam法の\ |beta|\ :sub:`2`）です。

.. describe:: classicalTry

 :デフォルト: 64

 |Delta|\ -NNPで使用する古典力場を最適化する際の繰り返し回数の上限です。

.. describe:: classicalLower

 :デフォルト: -50.0

 |Delta|\ -NNPで使用する古典力場を最適化する際、ここで指定した値(eV)よりも小さいエネルギーが現れにくくなるように、ペナルティ関数を適用して最適化します。

.. describe:: gpuThreads

 :デフォルト: 256

 （GPU版）CUDAのブロック当たりのスレッド数です。上限は1024（CUDAの仕様）です。32（ワープサイズ）の倍数を推奨します。

 GPU上で生成するスレッドに関する設定で、CPUのスレッド並列（OpenMP並列）とは関係ありません。

.. describe:: gpuAtomBlock

 :デフォルト: 512

 （GPU版）対称関数をGPUで計算するときに、ここで指定した数の原子ごとにまとめて処理を行います。

.. describe:: endProperty

 以降のファイル内容はコメントとして扱われます。

.. |angs| raw:: html

   &#8491;

.. |beta| raw:: html

   <em>&beta;</em>

.. |Delta| raw:: html

 &Delta;
 