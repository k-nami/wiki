# Paper notes

### [Yang+ 24](http://arxiv.org/abs/2402.17139), "Video as the New Language for Real-World Decision Making", arXiv (2024)

<!-- （かつ言語も扱うmultimodalな） -->
* 動画生成の基盤モデルについて動向まとめ
* 複数のcomputer visionタスク，テキストの質問に対する動画での回答，画像に基づく推論などを，LLMのように，単一モデルで扱える可能性がある．また，この基盤モデルは世界モデル，つまり，ゲームや身体モデル，科学的な物理法則等のシミュレータとも見なせる
* 課題は，データの少なさ，ラベルの不足，モデル選択，Hallucination，汎化性能
* 有望な研究方向は，動画版のRLHFや，動画を物理的な行動へgroundingしFBから自己改善させる方向を上げている
<!-- * 動画を扱う理由に，言語では取りこぼす情報があることを挙げているが，Hallucinationとは相性が悪いように見える． -->
<!-- * SORAなどの動向を踏まえると，この辺り，資本の力の限界を，2年後くらいに見れるかな？ -->

### [Liu+ 24](https://arxiv.org/abs/2405.16588), "Attaining Human`s Desirable Outcomes in Human-AI Interaction via Structural Causal Games", arXiv (2024)

![alt text](img/liu24.png)

* 2エージェント（AIと人間）間の相互作用をStructural Causal Gameで定式化
* 複数のNash均衡が存在する問題において，AI側に介入（pre-policy intervention）することで，人間の効用上，望ましい均衡になるよう誘導
* 実験はLLMベース．交渉問題と情報プロテクト
<!-- * 因果推論とゲーム理論を組み合わせたものは面白い -->
<!-- * 人間の効用が既知，Nash均衡が計算可能，介入量の学習アルゴリズムが非常にシンプルな点に改善の余地あり -->

### [Alakuijala+ 24](https://doi.org/10.48550/arXiv.2405.19988),"Video-Language Critic: Transferable Reward Functions for Language-Conditioned Robotics", arXiv (2024)

![alt text](img/Alakuijala24.png)

* タスク文字列と画像系列に対する非マルコフな報酬を自己教師あり学習
  * タスクを表す文字列cと動画vから，各時刻までの画像列とcの一致度を密な報酬関数（本文中はスコア）として学習
  * 訓練データには，行動や報酬ラベルが含まれず，動画がタスクを達成していればよい
  * TransformerベースのNNを，自己教師あり学習の枠組みで学習
  * ロスは二種類．ペアのvとcのスコアを上げ，ペアでないvとcのスコアを下げるcross-entorpyロスと，時刻方向に報酬が上がるよう促すsequential ranking loss
* 未学習のタスクやOpen-Xにおいて，学習速度が速く，タスク達成度が高いケースあり

### [Geist+ 22](https://arxiv.org/abs/2106.03787v4), "Concave Utility Reinforcement Learning: The Mean-Field Game Viewpoint", AAMAS (2022)

![alt text](img/Geist22.png)

* Concave Utility Reinforcement Learning(CURL)がMean-Feald Game(MFG)として解けるケースとそのアルゴリズム、GridWorldの実験をまとめている
  * CURLの目的関数は，状態行動対して定義される報酬を元にするのではなく，その確率分布に対する評価関数として広義に定義されるのがミソ
  * 同じ解法で模倣学習や多目的最適化、制約付き強化学習を扱える
* concaveなFを最大化する方策は、Fの偏微分を報酬とみなすMFGのナッシュ均衡に一致
* 仮想プレイとオンライン鏡像降下法で解いた場合の数値実験では、後者の学習速度が速い
<!-- * 非合理モデルが何処まで含まれるか？ -->

### [Barnes+ 23](https://arxiv.org/abs/2405.19988), "Massively Scalable Inverse Reinforcement Learning in Google Maps", arXiv (2024)

![alt text](img/Barnes23.png)

* 世界の道路ネットワーク（Google Map）で110Mの軌跡を使う大規模なIRL．決定的環境・割引率なし．
* MaxEntをベースに下記を組み合わせ
  1. 分割されたregionごとに報酬推定
  2. 状態価値の初期値をダイクストラ法で与える
  3. 状態行動分布を求めるために，二つの方策をmixした行動分布（demonstrationの状態分布からH step先まで確率的方策，その先から目的地状態までダイクストラ法で求めた決定的方策）を使用
  4. 道路ネットワークを圧縮
* demonstrationに対する一致度（特にAccuracy，IoU）で提案法が最も良い．学習された経路には，私有地でも通り抜け可能な経路が含まれた

### [Zhi-Xuan+ 24](http://arxiv.org/abs/2402.17930) “Pragmatic Instruction Following and Goal Assistance via Cooperative Language-Guided Inverse Planning.”, arXiv (2024)

* HumanとAssistantの2人ゲーム．2エージェントのjoint actionに関する共通のコストを最小化．
* ゴールとそれに対応する実行中の方策が不明な状況で，過去の行動履歴からベイズでゴールと方策を推定
* アルゴリズムのキモはwで，$P(g,\pi)$を計算している．
* wの定義で因果（Do）を考慮して，joint行動の尤度ではなく，humanの行動の尤度だけ考慮する点に工夫
* 実験環境でPDDLを用いている
* 発展的
  * PDDL，juliaでの実装あり．周辺問題の参考になりそう．

![Zhi-Xuan+ 24](img/Zhi-Xuan24.png)

### [Hao+ 22](https://proceedings.neurips.cc/paper_files/paper/2022/file/51ae7d9db3423ae96cd6afeb01529819-Paper-Conference.pdf) “Teacher Forcing Recovers Reward Functions for Text Generation.”, NeurIPS 2022

* Teacher Forcing（TF）が逆強化学習（IRL）の目的関数と等価であることを示し，学習コーパスから報酬を明示的に求めて，その報酬でRLする．
* コーパスがnon-parallelなことがミソ
* 疑問
  * 並列・非並列なコーパスとは？
  * TFの定義は「テキスト生成の文脈で，与えたテキストの尤度を最大化する問題」で正しい？

### [Li+ 24](https://aclanthology.org/2024.lrec-main.932.pdf) “LLMR: Knowledge Distillation with a Large Language Model-Induced Reward.”, Proceedings of the 2024 Joint International Conference on Computational Linguistics, Language Resources and Evaluation (LREC-COLING 2024)

* [Hao+ 22]を知識蒸留に利用
* 生徒モデルの出力を教師モデルに入力．間接的に教師モデル上の報酬を計算して，その報酬を用いたRLで生徒モデルを学習

### [Zhai+ 24](http://arxiv.org/abs/2405.10292), “Fine-Tuning Large Vision-Language Models as Decision-Making Agents via Reinforcement Learning.” arXiv, 2024

* VLMをRLでfine-tuning
* ミソは下記2つ
  1. VLMのinputにtask description，outputにCoTとActionがそれぞれ含む
  2. 行動の尤度（方策）を，CoTとActionに対応するトークン列ごとに項を分解し，スケールパラメータを導入（CoTとActionでトークン長が異なることを補う）
* RLHFと同様にタスクのデータでSFTをかけてからRLを実行しているのもポイント
* 実験規模(GPU memory)は80GB(A100x8)

![Zhai+ 24](./img/Zhai24.png)
