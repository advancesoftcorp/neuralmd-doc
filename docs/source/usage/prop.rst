.. _prop:

===================
sannp.propの書式
===================

\ :file:`sannp.prop`\ のテンプレートは、 :option:`sannp --temp` で出力できます。

行頭が#または!の行はコメントとして扱われます。

.. describe:: restart

 :デフォルト: 0

 0の場合、最初から学習を実行します。1の場合、ニューラルネットワークの情報を\ :file:`sannp.data`\ 、\ :file:`sannp.data_e`\ または\ :file:`sannp.data_q`\ から読み込み、学習を再開します。

.. describe:: withCharge

 :デフォルト: 0

 電荷の計算を行う(1)か行わない(0)かの指定です。

.. describe:: withHDNNP

 :デフォルト: 1

 系全体のエネルギーを教師データとするHDNNP法を使う(1)か、各原子に分割したエネルギーを教師データとするSANNP法を使う(0)かの指定です。

.. describe:: directSF

 :デフォルト: -1

 対称関数の正規化をミニバッチ内で行う(1)か、サンプル全体で行う(0)かの指定です。負の値を指定した場合、Behler対称関数ならサンプル全体、Many-Body対称関数ならミニバッチ内になります。

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

 :デフォルト: tanh

 ニューラルネットワークの活性化関数です。asis（使用しない）、sigmoid、tanh、eLUが指定できます。

.. describe:: lbfgs

 :デフォルト: 64

 学習時の最適化アルゴリズムの指定です。0を指定すると、Adam法を使用します。1以上の値を指定すると、その値を履歴数とするL-BFGS法を使用します。

.. describe:: batchs

 :デフォルト: 0

 学習時のミニバッチサイズです。0以下の値を指定した場合、ミニバッチは使用せず、フルバッチ（サンプル全体）で学習を行います。

.. describe:: epochs

 :デフォルト: 5000

 学習時の繰り返し回数の上限です。

.. describe:: renormLoss

 :デフォルト: 0

 損失関数の正規化を行う(1)か行わない(0)かの指定です。

.. describe:: rmseEnergy

 :デフォルト: 0.10

 学習が収束したか判定するためのエネルギー残差(RMS)の閾値(eV/atom)です。

.. describe:: rmseForce

 :デフォルト: 0.10

 学習が収束したか判定するための力の残差(RMS)の閾値(eV/\ |angs|\ )です。

.. describe:: rmseCharge

 :デフォルト: 0.01

 学習が収束したか判定するための電荷の残差(RMS)の閾値(e)です。

.. describe:: coefEnergy

 :デフォルト: 1.00

 エネルギーの損失関数のスケール係数(1/eV)です。

.. describe:: coefForce

 :デフォルト: 1.00

 力の損失関数のスケール係数(\ |angs|\ :sup:`2`/eV)です。

.. describe:: coefCharge

 :デフォルト: 1.00

 電荷の損失関数のスケール係数(1/e)です。

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

.. describe:: endProperty

 以降のファイル内容はコメントとして扱われます。

.. |angs| raw:: html

   &#8491;

.. |beta| raw:: html

   <em>&beta;</em>
