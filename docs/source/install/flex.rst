.. _flex:

============================
旧ライセンスの使用方法
============================

これまで、NeuralMDではFlexNetライセンスを利用していましたが、バージョン2.0以降ではSentinelライセンスへと移行します。

バージョン2.0より前のバージョンでは引き続きFlexNetライセンスをご利用いただくことができますが、バージョン2.0以降にリリースされるバージョンでは、FlexNetライセンスは利用できません。

FlexNetライセンスをご利用の場合のみこちらのマニュアルをご参照ください。

.. _flexw:

ライセンスの設定(Windows)
=============================

ライセンスファイル( :file:`neumd.lic` )をインストール先の :file:`license` フォルダーに格納してください。

実行時には、環境変数 :envvar:`ADVANCED_LICENSE_FILE` にライセンスファイルのパスが設定されている必要があります。他の弊社製品をお使いの場合、既に環境変数の値が設定されている場合がありますが、その場合はセミコロン区切りでパスを追加してください。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 set ADVANCED_LICENSE_FILE=%ADVANCED_LICENSE_FILE%;C:\Program Files\AdvanceSoft\NeuralMD\license\neumd.lic

.. _flexl:

ライセンスの設定(Linux)
=============================

FlexNetライセンスによるライセンス認証を行うには、ライセンスサーバー（ライセンス認証用のプログラム）を起動しておく必要があります。

.. _launchlfromwin:

Windowsから操作する場合
-----------------------

弊社ツール\ `remoteLicense <https://remotelicense-doc.readthedocs.io/ja/latest/>`_\ を使うことで、Windows上から簡単にLinuxマシン上のライセンスサーバーを起動することができます。

手順の概要を以下に示します。詳細は\ `マニュアル <https://remotelicense-doc.readthedocs.io/ja/latest/>`_\ を参照してください。

#. `remoteLicenseインストーラー <https://remotelicense-doc.readthedocs.io/ja/latest/install.html#download>`_\ を使ってインストールし、起動します。
#. :guilabel:`Host` タブでLinuxマシンへの接続情報を設定します。
#. :guilabel:`License` タブでライセンスファイル( :file:`neumd.lic` )を選択します。
#. :guilabel:`Start` タブの :guilabel:`Execute \`lmgrd'` ボタンをクリックすると、ライセンスサーバーが起動します。

.. _launchlonlinux:

Linux上で操作する場合
-----------------------

ライセンスファイル( :file:`neumd.lic` )をインストール先の :file:`license` ディレクトリにコピーしてください。

ライセンスサーバーの実行ファイルはインストール先の :file:`license/lmgrd` です。端末（ターミナル）でインストール先のディレクトリに移動したら、以下のコマンド例のように起動します。

.. code-block:: console

 $ license/lmgrd -c license/neumd.lic -l license/lmgrd.log

ライセンスサーバーの状態を表示するには、インストール先の :file:`license/lmstat` を使用します。

.. code-block:: console

 $ license/lmstat -a -c license/neumd.lic

また、ライセンスサーバーを終了するには、インストール先の :file:`license/lmdown` を使用します。

.. code-block:: console

 $ license/lmdown -c license/neumd.lic

実行時には、環境変数 :envvar:`ADVANCED_LICENSE_FILE` にライセンスファイルのパスが設定されている必要があります。

.. code-block:: console
 :caption: デフォルトの場所にインストールした場合の例

 export ADVANCED_LICENSE_FILE=/opt/AdvanceSoft/NeuralMD/license/neumd.lic

.. _flexfloating:

フローティングライセンス
-----------------------------

Linuxマシンに対して発行されたFlexNetライセンスはフローティングライセンスとなっており、ネットワーク接続された別のマシン上でNeuralMDを使うことができます。

- ライセンスサーバーとして使うマシン側

 remoteLicenseを使うか、またはマシン上で直接ライセンスサーバーを起動してください。

- NeuralMDを使うクライアントマシン側

 ライセンスサーバーと同じライセンスファイルを、NeuralMDのインストール先の :file:`license` ディレクトリにコピーし、ファイルのパスを :envvar:`ADVANCED_LICENSE_FILE` に設定してください。

ライセンス認証がうまくいかない場合は、以下をご確認ください。

- クライアントからライセンスサーバーへの接続には、ライセンスファイル中に書かれたホスト名を使用します。ホスト名を使った接続ができない場合、ファイル中のホスト名をIPアドレスに書き換えることで接続できるようになることがあります。

- ライセンスサーバー起動中はライセンスマネージャーデーモン :file:`lmgrd` とベンダーデーモン :file:`advanced` の2つのプロセスが起動し、それぞれがネットワーク通信を行います。使用するポート番号は動的に決まります（\ :file:`lmgrd` は27000-27009番ポートを使用）が、ファイアウォールの設定等のためにポート番号を固定したい場合は、ライセンスファイル中に追記して指定することができます。

- ライセンスファイルはテキストファイルですので通常のテキストエディタで編集できます。ホスト名の変更・ポート番号の追記でライセンスの再発行は必要ありません。サーバー側・クライアント側両方で同じように変更してください。

 .. table::

  +-----------------------------------------------------------------------------+
  | lmgrdが30000番、advancedが30001番ポートを使うように設定する例               |
  +=============================================================================+
  || SERVER (ホスト名) COMPOSITE=(ホストID) |portlmgrd|                         |
  || VENDOR advanced |portadvanced|                                             |
  || USE_SERVER                                                                 |
  || FEATURE ...                                                                |
  +-----------------------------------------------------------------------------+

.. |portlmgrd| raw:: html

   <font color="blue">30000</font>

.. |portadvanced| raw:: html

   <font color="blue">PORT=30001</font>

.. _concatlicense:

ライセンスファイルの結合
-------------------------------

本製品とは別の弊社製品をお使いの場合、ライセンスファイルを結合することで、1つのライセンスサーバーで複数製品のライセンス認証が可能です。\ :file:`.lic`\ ファイルを単純に結合した後、重複する行（ ``SERVER`` 、 ``VENDOR`` 、 ``USE_SERVER`` ） を削除して、1つのライセンスファイルを作ります。その後、そのファイルを使ってライセンスサーバーを起動してください。

.. table::
 :widths: 100,100,10,100

 +------------------------------------+---------------------------------------++-------------------+
 | ファイル1.lic                      | ファイル2.lic                         || 結合ファイル.lic  |
 +====================================+=======================================++===================+
 || SERVER ...                        || SERVER ...                           ||| SERVER ...       |
 || VENDOR ...                        || VENDOR ...                           ||| VENDOR ...       |
 || USE_SERVER                        || USE_SERVER                           ||| USE_SERVER       |
 || |featgreen|                       || |featblue|                           ||| |featgreen|      |
 || |nbsp|                            || |nbsp|                               ||| |featblue|       |
 +------------------------------------+---------------------------------------++-------------------+

.. |featblue| raw:: html

   <font color="blue">FEATURE ...<br>...</font>

.. |featgreen| raw:: html

   <font color="green">FEATURE ...<br>...</font>

.. |nbsp| raw:: html

   &nbsp;<br>&nbsp;

