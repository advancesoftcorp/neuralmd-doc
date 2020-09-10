.. _theory:

============================
理論
============================

対称関数
------------------

原子が置かれている状況（具体的には、近傍の原子構造）をニューラルネットワークに入力できる形に変換するのが、対称関数です。原子のエネルギーは系の平行移動や回転、同種原子の入れ替えに対して不変（対称）であるため、エネルギーに関する特徴量を抽出する対称関数に対しても、同じ性質が要請されます。また、ニューラルネットワークの入力ノード数が固定なので、対称関数の値の数も近傍原子数に依らず一定である必要があります。例えば「近傍原子との距離」や「近傍原子のなす角度」は、平行移動や回転に対しては不変ですが、近傍原子数が増えればその数だけ値が増えるため、そのまま使うことはできません。

対称関数としては様々なものが提案されていますが、本製品ではBehlerが提案\ [1]_\ した対称関数（\ :math:`G^2`\ ：動径成分、\ :math:`G^3`\ または\ :math:`G^4`\ ：角度成分）：

.. math::

 f_c(R_{ij}) &=
 \begin{cases}
 \tanh^3\left[ 1 - \frac{R_{ij}}{R_c} \right] \text{or} \; \frac{1}{2}\left[ \cos\left(\frac{\pi R_{ij}}{R_c}\right)+1 \right] &\text{for} \; R_{ij}\leq R_c \\
 0 &\text{for} \; R_{ij} > R_c,
 \end{cases} \\
 G_i^2 &= \sum_{j=1}^{N_\text{atom}} e^{-\eta (R_{ij}-R_s)^2} \cdot f_c(R_{ij}), \\
 G_i^3 &= 2^{1-\zeta} \sum_{j \neq i} \sum_{k \neq i,j} \left[ (1+\lambda \cdot \cos\theta_{ijk})^\zeta \cdot e^{-\eta \left[ (R_{ij}-R_s)^2+(R_{ik}-R_s)^2+(R_{jk}-R_s)^2 \right]} \cdot f_c(R_{ij}) \cdot f_c(R_{ik}) \cdot f_c(R_{jk}) \right], \\
 G_i^4 &= 2^{1-\zeta} \sum_{j \neq i} \sum_{k \neq i,j} \left[ (1+\lambda \cdot \cos\theta_{ijk})^\zeta \cdot e^{-\eta \left[ (R_{ij}-R_s)^2+(R_{ik}-R_s)^2 \right]} \cdot f_c(R_{ij}) \cdot f_c(R_{ik}) \right],

Chebyshev多項式を使った対称関数\ [2]_\ （\ :math:`c_\alpha^{(2)}`\ ：動径成分、\ :math:`c_\alpha^{(3)}`\ ：角度成分）：

.. math::

 T_0(x) &= 1, \\
 T_1(x) &= x, \\
 T_{n+1}(x) &= 2xT_n(x)-T_{n-1}(x), \\
 c_\alpha^{(2)} &= \sum_{j \neq i} T_\alpha \left(\frac{2R_{ij}}{R_c}-1 \right)f_c(R_{ij}), \\
 c_\alpha^{(3)} &= \sum_{j \neq i} \sum_{k \neq i,j} T_\alpha \left(\frac{2\theta_{ijk}}{\pi}-1 \right)f_c(R_{ij})f_c(R_{ik}),


および、SANNPの論文\ [3]_\ で使われている対称関数（\ :math:`G^{(2)}`\ ：2体成分、\ :math:`G^{(3)}`\ ：3体成分）：

.. math::

 R_\alpha^k &= R_\text{inner} + (\alpha - 1)h_k, \\
 \text{where} \; \alpha &= 1,2,...,M_k, \\
 \varphi_\alpha^{(k)}(R_{ml}) &= 
 \begin{cases}
 \frac{1}{2}\cos\left(\frac{R_ml-R_\alpha^k}{h_k}\pi\right) + \frac{1}{2}, & | R_{ml}-R_\alpha^k | < h_k\\
 0, & \text{Otherwise},
 \end{cases} \\
 G_{\alpha,l}^{(2)} &= \sum_m \varphi_\alpha^{(2)}(R_{ml}), \\
 G_{\alpha\beta\gamma,l}^{(3)} &= \sum_{m,n} \varphi_\alpha^{(3)}(R_{ml})\varphi_\beta^{(3)}(R_{nl})\varphi_\gamma^{(3)}(R_{mn}), \\
 h_k &= (R_\text{outer}-R_\text{inner})/M_k

（本製品ではMany-Body対称関数と呼びます）が使用可能です。

系に異なる複数の元素が含まれる場合には、元素の組み合わせごとに対称関数\ :math:`\phi`\ （のパラメータセット）を用意することでその違いを表現する、という方法があります：

.. math::

 G_i^2 &= \sum_{j \neq i} \phi_{Z_i, Z_j}^{(2)} (\mbox{\boldmath $r$}_i, \mbox{\boldmath $r$}_j) ,\\
 G_i^3 &= \sum_{j \neq i} \sum_{k \neq i,j} \phi_{Z_i, Z_j, Z_k}^{(3)} (\mbox{\boldmath $r$}_i, \mbox{\boldmath $r$}_j, \mbox{\boldmath $r$}_k) .

この方法では、系に含まれる元素の種類が増えると、その組み合わせの数だけ多数のパラメータが必要になってしまう、という問題があります。これを避けるため、対称関数は増やさずに共通のものを使い、かわりに元素ごとの「重み」を導入するという方法\ [2]_\ [4]_\ が提案されています：

.. math::

 G_i^2 &= \sum_{j \neq i} g(Z_j) \phi^{(2)} (\mbox{\boldmath $r$}_i, \mbox{\boldmath $r$}_j) ,\\
 G_i^3 &= \sum_{j \neq i} \sum_{k \neq i,j} h(Z_j, Z_k) \phi^{(3)} (\mbox{\boldmath $r$}_i, \mbox{\boldmath $r$}_j, \mbox{\boldmath $r$}_k) .

本製品では、Behler対称関数とChebyshev対称関数でこの重み付き対称関数が使用可能です。重みとしては、原子番号 :math:`g(Z_i)=Z_i` およびその幾何平均 :math:`h(Z_i, Z_j)=\sqrt{Z_i Z_j}` を使用します。

ニューラルネットワーク
-----------------------------

ニューラルネットワークは、入力層、1つ以上の中間層（隠れ層）、出力層からなります。各層には1つ以上のノードがあり、前の層のノードの値を入力として、次の層のノードの値を決めます。

層 :math:`i` のノード :math:`j` の値を :math:`x_j^i` とすると、

.. math::

 x_j^{i+1} = f\left(\sum_k w_{jk}^i \cdot x_k^i + b_k^i\right)

となります。重み :math:`w` 、バイアス :math:`b` がニューラルネットワークのパラメータで、求める出力を得るために\ :math:`w`\ 、\ :math:`b`\ を最適化するのが「ニューラルネットワークの学習」ということになります。

重みとバイアスに加え、非線形な振る舞いを表現するために使われるのが活性化関数 :math:`f` です。本製品では活性化関数として

- シグモイド関数 :math:`f(x)=1/(1+e^{-x})` 
- tanh関数 :math:`f(x)=\tanh(x)`
- eLU関数 :math:`f(x)=x \; \text{for} \; x \geqq 0; \; e^x - 1 \; \text{for} \; x < 0`

または活性化関数なし :math:`f(x)=x` が選べます。

SANNP
--------------

構造中のある原子 :math:`i` に対して、近傍の構造から対称関数を計算し、ニューラルネットワークに入力すると、出力としてその原子のエネルギー :math:`E_i^\text{NN}` が得られます。

一方、教師データとしては密度汎関数理論(DFT)に基づく第一原理計算が使われますが、その結果は「各原子のエネルギー」という形にはなっていません。系の全エネルギー :math:`E_\text{tot}^\text{DFT}` を使い、 :math:`|E_\text{tot}^\text{DFT} - \sum_i E_{i}^\text{NN}|` を残差として最適化を行う方法がありますが、この場合1つの原子構造に対するDFT計算からエネルギーに関する情報は1つしか得られないことになります。

本製品で採用しているSANNP(Single Atom Neural Network Potential)\ [3]_\ では、DFT計算の結果を各原子のエネルギー :math:`E_i^\text{DFT}` に分割する手法\ [5]_\ を使うことで、各原子のエネルギーを残差 :math:`|E_i^\text{DFT} - E_i^\text{NN}|` を使って直接最適化しています。これにより、1つの原子構造に対するDFT計算から得られるエネルギーに関する情報が原子数倍になり、少ないDFT計算の結果からでも効率よく学習を行うことができます。

また、エネルギーと同様に、ニューラルネットワークを使って「各原子の電荷」を得ることができます。クーロン相互作用は長距離でも働くため、クーロン相互作用も含めてニューラルネットワーク力場で計算しようとすると大きなカットオフ半径が必要になります。本製品ではニューラルネットワーク力場と、ニューラルネットワークで計算した電荷を使ったクーロン相互作用を組み合わせて使うことができますので、短距離の相互作用を扱うニューラルネットワーク力場の部分についてはカットオフ半径を小さくして計算することが可能です。

系の全エネルギーは、ニューラルネットワークで計算した各原子のエネルギー :math:`E_i^\text{NN}` を使って、電荷を使わない場合

.. math::

 E_\text{tot}^\text{NN} = \sum_i E_i^\text{NN}

と表現されます。また、ニューラルネットワークで計算した電荷 :math:`Q_i^\text{NN}` を使う場合は、系の全電荷が0になるようシフトした上で、

.. math::

 E_\text{tot}^\text{NN} &= E_\text{short} + E_\text{elec} \\
 &= \sum_i E_i^\text{NN} + \sum_i \sum_{j>i}^{R_{ij} < R_\text{elec}} \frac{Q_i^\text{NN} Q_j^\text{NN}}{4\pi\epsilon_0R_{ij}} \cdot f_\text{screen}(R_{ij}), \\
 f_\text{screen}&(R_{ij}) =
 \begin{cases}
 \frac{1}{2}\left[1-\cos\left(\frac{\pi \cdot R_{ij}}{R_\text{short}}\right)\right] \; & \text{for} \; R_{ij} \leq R_\text{short} \\
 1 & \text{for} \; R_{ij} > R_\text{short}
 \end{cases}

として計算します。

.. [1] "Constructing high‐dimensional neural network potentials: A tutorial review", J. Behler, *Int. J. Quantum Chem.* **115**, 1032-1050 (2015). DOI: `10.1002/qua.24890 <https://doi.org/10.1002/qua.24890>`_
.. [2] "Efficient and accurate machine-learning interpolation of atomic energies in compositions with many species", N. Artrith *et al.*, *Phys. Rev. B* **96**, 014112 (2017). DOI: `10.1103/PhysRevB.96.014112 <https://doi.org/10.1103/PhysRevB.96.014112>`_
.. [3] "Density functional theory based neural network force fields from energy decompositions", Y. Huang *et al.*, *Phys. Rev. B* **99**, 064103 (2019). DOI: `10.1103/PhysRevB.99.064103 <https://doi.org/10.1103/PhysRevB.99.064103>`_
.. [4] "wACSF—Weighted atom-centered symmetry functions as descriptors in machine learning potentials", M. Gastegger *et al.*, *J. Chem. Phys.* **148**, 241709 (2018). DOI: `10.1063/1.5019667 <https://doi.org/10.1063/1.5019667>`_
.. [5] "First-principles green-Kubo method for thermal conductivity calculations", J. Kang and L.-W. Wang, *Phys Rev B* **96**, 020302(R) (2017). DOI: `10.1103/PhysRevB.96.020302 <https://doi.org/10.1103/PhysRevB.96.020302>`_