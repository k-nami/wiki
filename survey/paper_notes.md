# Paper notes

テスト試行中．

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
