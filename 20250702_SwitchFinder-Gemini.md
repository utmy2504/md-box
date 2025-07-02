# SwitchFinderの全貌：RNA構造スイッチ探索の新時代を切り拓く計算フレームワーク

2024年7月に科学誌「Nature Methods」で発表された論文は、遺伝子発現を制御するRNAの動的な立体構造変化、すなわち「**RNA構造スイッチ**」を、ゲノム情報から網羅的に発見するための画期的な計算フレームワーク「**SwitchFinder**」を世に示しました。本稿では、この先進的なツールについて、その概要、技術的背景、具体的な使用法、そして現在判明している問題点をまとめます。

---

### ## 1. SwitchFinderとは？

SwitchFinderは、既知のRNAスイッチの配列情報に頼ることなく、RNAの塩基配列のみから、機能未知のRNA構造スイッチを全く新たに（*de novo*で）発見することを目的とした計算フレームワークです。

#### **基本原理：フォールディングエネルギーランドスケープの解析**

その核心は、RNAが取りうる無数の立体構造とその安定度を表現した「**フォールディングエネルギーランドスケープ**」の解析にあります。このエネルギーの地形図において、SwitchFinderは**二つの安定な構造（エネルギーの「谷」）が、比較的低いエネルギー障壁によって隔てられている**特徴的な地形を持つ配列を探索します。このような地形は、二つの異なる立体構造間を比較的容易に行き来できる「スイッチ」としての潜在能力を示唆します。

---

### ## 2. SwitchSeeker：予測と実験を統合する発見のフレームワーク

SwitchFinderは単独のツールではなく、論文で提唱されたより大きな発見基盤「**SwitchSeeker**」の中核をなす計算エンジンです。SwitchSeekerは、計算による予測と、最先端の実験技術による検証を反復的に組み合わせることで、予測の信頼性を飛躍的に高めます。

* **実験的検証技術**: SwitchFinderによる予測を検証するために、細胞内のRNA構造を直接解析する**DMS-MaPseq**や、立体構造を可視化する**クライオ電子顕微鏡（Cryo-EM）**といった技術が駆使されます。

---

### ## 3. ケーススタディ：自己免疫疾患の鍵「RORC RNAスイッチ」

SwitchFinderの威力を示す象徴的な例が、**RORC遺伝子**の3'非翻訳領域（3'UTR）で発見されたRNAスイッチです。

* **生物学的重要性**: RORC遺伝子は、過剰に活性化すると多発性硬化症などの自己免疫疾患を引き起こす**Th17免疫細胞**のマスター制御因子です。
* **発見された制御機構**: このRNAスイッチは、自身の構造を変化させることで、細胞の品質管理機構である**ナンセンス変異依存mRNA分解（NMD）**経路による分解を制御し、RORCタンパク質の生産量を巧みに調節していました。

---

### ## 4. 実践ガイド：インストールから実行まで

SwitchFinderは、Dockerというコンテナ技術を用いて利用するのが最も簡単です。

#### **ステップ1: Dockerイメージの取得**
まず、ターミナルで以下のコマンドを実行し、SwitchFinderの環境をダウンロードします。

    docker pull goodarzilaborder/switch_finder_image:latest

#### **ステップ2: Dockerコンテナの起動と接続**
次に、解析したいFASTAファイルがあるフォルダに`cd`で移動してから、以下のコマンドでコンテナを起動します。これにより、現在のフォルダがコンテナ内の`/switchfinder`に接続されます。

    docker run -it -v $(pwd):/switchfinder goodarzilaborder/switch_finder_image:latest

#### **ステップ3: SwitchFinderのインストール**
コンテナの中に入ったら、以下のコマンドで最終的なインストールを行います。

    pip install git+https://github.com/goodarzilab/SwitchFinder.git

#### **ステップ4: SwitchFinderの実行**
インストール後、`python`コマンドでスクリプトのフルパスを指定して実行します。

    python /opt/conda/envs/switch_finder_env/lib/python3.11/site-packages/SwitchFinder/wrappers/SwitchFinder_pipeline.py \
        --input_fastafile <あなたのFASTAファイル名.fa> \
        --out ./output_folder \
        --temp_folder ./temp_folder \
        --RNAstructure_path /programs/RNAstructure \
        --RNApathfinder_path /programs/RNApathfinder

---

### ## 5. 実行時エラーとデバッグの要約
このツールは、環境や入力データによっていくつかの特徴的なエラーを発生させることがあります。以下に、解決済みの問題とその対策を要約します。

* **問題: `.../partition: not found` エラー、およびそれに続く `ValueError`**
    * **原因**: プログラム内部のバグ。依存プログラム(`RNAstructure`)のパスが、開発者のPC環境のパス (`/avicenna/khorms/...`) に固定（ハードコード）されているため。
    * **対策**: `--RNAstructure_path`と`--RNApathfinder_path`オプションで、コンテナ内の正しいプログラムの場所（`/programs/RNAstructure`など）を明示的に指定する。

* **問題: `FileNotFoundError`**
    * **原因**: Dockerコンテナ内から、Mac本体のファイルパスを指定している。コンテナは接続されたフォルダ以外にアクセスできない。
    * **対策**: 入力ファイルはコンテナに接続したフォルダに置き、コンテナ内のパス（例: `--input_fastafile all_is.fa`）で指定する。

* **問題: `command not found` または `Permission denied`**
    * **原因**: スクリプトのパスが通っていない、または実行権限がない。
    * **対策**: `python`コマンドでスクリプトのフルパスを指定して実行する。

---

### ## 6. その他の既知の問題と対策

* **メモリ不足 (`Killed`)**: 入力配列が多い、または長い場合にメモリを使い果たして強制終了することがあります。Docker Desktopの**設定 > Resources > Memory**で使用メモリを増やすか、入力FASTAファイルを分割して処理してください。
* **計算エラー (`ValueError: negative dimensions are not allowed`)**: 非常に短い配列が入力に含まれていると、計算の最終段階でエラーが出ることがあります。あらかじめ入力FASTAファイルから**短い配列（例: 100bp以下）を除外**することで回避できる場合があります。
