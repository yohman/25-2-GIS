# 社会貢献のための地図作り: 地図で語るストーリー  
（オンライン教材: [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)）

---

## Week 1: オリエンテーション、VS Codeのインストール、GitHubの基本
- **講義:**
  - GISと空間データサイエンスとは何か
  - ウェブマッピングと実社会での活用例
  - バージョン管理の概要と、共同開発におけるGitHubの重要性
  - Visual Studio Code (VS Code) の開発環境としての紹介
- **実践:**
  - Visual Studio Codeをインストール
  - GitHubアカウントとリポジトリを設定
  - VS CodeをGitHubにリンクし、基本的なワークフロー（commit、push、pull）を練習
- **課題:**
  1. **GitHubリポジトリを作成**する（ここに毎週の提出物と最終プロジェクトをまとめていく）
  2. **興味のあるウェブ地図を1つ見つけ**、そのスクリーンショットを撮る
  3. 新しいGitHubリポジトリに **Week01.md** というMarkdownファイルを作成し、以下を記述:
     - 地図の紹介（名前やテーマなど）
     - **スクリーンショット**と**元サイトへのリンク**を含める
     - 地図の簡単な**評価**（長所、短所、デザイン、機能面など）
  4. **Week01.md** をコミットして、リポジトリにプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第1章～2章（環境構築や基本的なプロジェクト構成）

---

## Week 2: MapLibre GLを使った初めてのウェブ地図作成とオンライン公開
- **講義:**
  - MapLibreの概要: どのようにオープンソースのウェブマッピングを強化するか
  - GitHub Pagesの概要: 公開ウェブプロジェクトをホスティングする方法
  - HTML、CSS、JavaScriptの基本: シンプルなウェブ地図を構築するための構成・スタイリング・スクリプト
- **実践:**
  1. MapLibre GL JSをCDNまたはnpmで導入（アカウントやAPIトークンは不要）
  2. 地図のプレースホルダーを含む基本的なHTMLファイルをVS Codeのプロジェクトに作成
  3. MapLibre GLを使い、カスタムの中心座標・ズーム・スタイルを設定したシンプルな地図を作る
  4. GitHub Pagesでウェブページを公開
  5. 公開した地図を複数のデバイスでテスト
- **課題:**
  1. **シンプルな地図をGitHub Pagesで公開**し、リポジトリのREADMEにライブURLを記載する
  2. このコースの**最終プロジェクトのアイデアを提案**する。**Week02.md** という新しいファイルを作り、以下を記述:
     - 選んだ**テーマ**や課題（社会的、環境的、その他）
     - 想定される**データセット**やデータソース
     - プロジェクトの**目的**や意義（誰に役立つのか、どのように役立つのか）
  3. **Week02.md** をコミットしてプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第2章～3章（基本的な地図表示とセットアップ）

---

## Week 3: 地図へのデータレイヤーの追加
- **講義:**
  - GeoJSONの基本: 構造、形式、マッピングでの活用例
  - MapLibre GLでデータレイヤーを読み込み、スタイル設定する方法
- **実践:**
  - GeoJSONデータセット（ポイント、ライン、ポリゴンなど）を地図に追加
  - MapLibre GLのプロパティ（色、サイズ、不透明度など）を使ってスタイリング
- **課題:**
  1. **関連するデータレイヤー**（GeoJSONなど）を**最終プロジェクトの地図**に統合する
  2. **Week03.md** に以下を記載:
     - このデータセットを選んだ理由
     - 選択した**スタイリング設定**（色、マーカーなど）
     - GitHub Pagesのデプロイを更新し、新しい地図バージョンへのリンクを記載
  3. **Week03.md** をコミットしてプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第4章（レイヤーの追加とスタイリング）

---

## Week 4: MapLibre GLによるインタラクティブ機能の強化
- **講義:**
  - JavaScriptを活用した地図との対話: ポップアップ、ツールチップ、クリックイベント
  - ズームボタン、トグル、ナビゲーション補助などのユーザーフレンドリーなコントロール
- **実践:**
  - 地図にポップアップやツールチップを追加し、情報を表示
  - インタラクティブなコントロールを実装し、地図の使いやすさを向上
- **課題:**
  1. **少なくとも1つのインタラクティブ機能**（ポップアップ、ツールチップ、ホバー効果など）を最終プロジェクトの地図に追加
  2. **Week04.md** に以下を記載:
     - 追加した**インタラクティブ要素**の概要
     - これらの要素がユーザーにどのようにデータ/ストーリーを理解させるか
     - 更新したGitHub Pagesリンク
  3. **Week04.md** をコミットしてプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第5章（インタラクションとイベント処理）

---

## Week 5: 地図スタイルとテーマのカスタマイズ
- **講義:**
  - MapLibre GLにおけるカスタムスタイル設定: スタイルJSONの仕組み
  - テーマ別で視覚的に魅力的な地図を設計するためのベストプラクティス
- **実践:**
  - JSONファイルの直接編集、または外部スタイルエディタを使ったカスタム地図スタイルの作成
  - MapLibre GLマップへのカスタムスタイル適用
- **課題:**
  1. **カスタムスタイルを導入または作成**し、最終プロジェクトの地図に適用
  2. **Week05.md** に以下を記載:
     - **スタイル選択の理由**（カラースキーム、フォント、ベースマップなど）
     - これらのデザイン変更が可読性や美観にどう貢献するか
     - 更新した地図のリンク
  3. **Week05.md** をコミットしてプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第6章（スタイル管理とカスタマイズ）

---

## Week 6: ストーリーマップの作成
- **講義:**
  - ストーリーマップとは何か：影響力のあるストーリーマップの事例
  - ナラティブとビジュアル要素を組み合わせるためのベストプラクティス
- **実践:**
  - 地図にテキストや画像、メディアを追加してナラティブを構築
  - HTMLとCSSを使って、ビジュアルとストーリーテリングを融合させたページレイアウトをデザイン
- **課題:**
  1. **ストーリーテリング要素**（テキストブロック、画像、メディアなど）の導入を開始
  2. **Week06.md** に以下を記載:
     - **ナラティブ構成**の概要（導入、主要ポイント、結論）
     - ユーザーをどのように**ストーリーに導く**か
     - 更新したGitHub Pagesリンク
  3. **Week06.md** をコミットしてプッシュする

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - コンテンツ/オーバーレイやレイアウトに関する章を参照

---

## Week 7: 中間発表の準備
- **講義:**
  - 技術系プロジェクトを効果的に発表するためのヒント
  - 地図の目的、デザイン、インタラクティブ性を説明する方法
- **実践:**
  - 中間発表に向けて地図およびプロジェクトページを洗練
  - ピアとの発表練習を行い、フィードバックを取り入れる
- **課題:**
  1. 中間レビューを想定し、**最終プロジェクト地図を磨き上げる**
  2. **Week07.md** に以下を記載:
     - これまでの**進捗**の要約
     - ピアレビューを受けたい**課題**や**疑問点**
     - 更新した地図へのリンク
  3. **Week07.md** をコミットしてプッシュ

**参考教材の関連章**  
- 新規の技術チュートリアルはなし：洗練とレビューに注力

---

## Week 8: 中間発表
- **アクティビティ:**
  - 進行中のプロジェクトを学生が発表
  - ピアや講師からのフィードバックを受け、改善点を確認
- **課題:**
  1. 発表後に得た**主要なフィードバック**を地図に反映
  2. リポジトリおよび地図のリンクを必要に応じて更新

**参考教材の関連章**  
- 必要に応じて適宜参照

---

## Week 9: 美しくユーザーフレンドリーな地図のデザイン
- **講義:**
  - デザインの重要性：コンテンツよりデザインが重要になる場合もある理由
  - 色やフォント、レイアウトの選択でユーザーエクスペリエンスを向上
  - ボタン、ドロップダウン、コントロールを直感的にデザイン
  - ウェブ地図のアクセシビリティ：視覚障害者を含む全てのユーザーに配慮
- **実践:**
  - 地図レイアウトを改善し、デザイン要素を向上
  - 凡例、ボタン、ドロップダウンなどのUIコンポーネントを追加・強化
- **課題:**
  1. 最終プロジェクト地図の**UI/UX**とアクセシビリティを向上させる
  2. **Week09.md** に以下を記載:
     - 導入した**UI/UX改善点**（凡例、ボタン、アクセシビリティ対策など）
     - それらがユーザーエクスペリエンスにどう寄与するか
     - 更新した地図のリンク
  3. **Week09.md** をコミットしてプッシュ

**参考教材の関連章**  
- UI/UXに関する章（該当があれば）を確認

---

## Week 10: 高度なマッピング技術
- **講義:**
  - MapLibre GLの高度機能：クラスタリング、ヒートマップ、カスタムマーカー
  - ストーリーテリングを強化するための高度機能の使いどころと方法
- **実践:**
  - 密集データの可視化のためにクラスタリングやヒートマップを追加
  - カスタムマーカーや高度なインタラクティブ性を試す
- **課題:**
  1. 最終プロジェクトに**少なくとも1つの高度可視化機能**（クラスタリング、ヒートマップ、カスタムマーカーなど）を統合
  2. **Week10.md** に以下を記載:
     - この高度機能が地図のメッセージ性や使いやすさをどのように向上させるか
     - 直面した**技術的課題**など
     - 最新のGitHub Pagesリンク
  3. **Week10.md** をコミットしてプッシュ

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - 第7章（クラスタリング、ヒートマップなど）

---

## Week 11: 空間分析とデータのフィルタリング
- **講義:**
  - 空間分析の概要：動的なデータフィルタリング
  - 時間ベースやカテゴリーベースでのフィルタリング事例
- **実践:**
  - スライダーやドロップダウンを使って地図上のフィルターや動的分析機能を実装
  - 空間的パターンを動的に可視化
- **課題:**
  1. 最終プロジェクトに**フィルターまたは分析機能**を追加（例：時間スライダー、カテゴリーフィルター）
  2. **Week11.md** に以下を記載:
     - フィルター/分析の**ロジック**の説明
     - ユーザーがどのようにインタラクションして情報を得られるか
     - 更新後の地図リンク
  3. **Week11.md** をコミットしてプッシュ

**参考教材の関連章**  
- [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b)  
  - フィルタリングや動的データ処理に関する章を参照

---

## Week 12: 地図の最終調整と公開
- **講義:**
  - 公開に向けた地図の最適化：アクセシビリティ、パフォーマンス、モバイル対応
  - 最終公開前のテストとデバッグ
- **実践:**
  - 地図のスタイルやインタラクティブ性を最終調整
  - 複数のデバイスやブラウザでテスト
- **課題:**
  1. **複数デバイス（デスクトップ、モバイル、タブレットなど）で最終テスト**を実施
  2. **Week12.md** に以下を記載:
     - 発見した**バグ**とその**解決策**
     - マップのパフォーマンスやレスポンシブ対応の確認結果
     - 最新のGitHub Pagesリンク
  3. **Week12.md** をコミットしてプッシュ

**参考教材の関連章**  
- パフォーマンスや最適化に関する章があれば参照

---

## Week 13: プロジェクトの調整とフィードバック
- **講義:**
  - プロジェクトの目標を再確認し、想定した効果に合致しているか確認
  - デバッグと仕上げ
- **実践:**
  - 最終的な細部の調整、ユーザビリティテスト
  - ピアレビューとフィードバックの実施
- **課題:**
  1. プレゼンテーションに向けた**最終地図の準備**
  2. **Week13.md** に以下を記載:
     - 行った**最終調整**（デザイン、データ、機能など）
     - 直前に行った修正や改善点
     - 発表間近のGitHub Pagesリンク
  3. **Week13.md** をコミットしてプッシュ

**参考教材の関連章**  
- 必要に応じて前の章を再確認

---

## Week 14: 最終発表
- **アクティビティ:**
  - 学生が完成したウェブ地図を発表
  - プロジェクトの影響度、技術的課題、学んだことにフォーカス
- **フィードバック:**
  - グループおよびインストラクターからの講評で強みと改善点を確認

- **課題（発表後）:**
  1. 最終発表後、リポジトリのメインREADMEを更新:
     - プロジェクトの**最終まとめ**
     - 得られた**学び**や今後の改善点
     - ライブマップのリンクが機能していることを確認
  2. **お疲れさまでした！** これで機能的かつ有益なウェブ地図を完成させました。

---

## 主な調整点
- **MapLibre GLの使用:**  
  すべてのハンズオンでMapLibre GL JSを使用し、追加のアカウントやAPIトークンは不要。  
- **新しいオンライン教材:**  
  [【初級編】MapLibre GL JSを使ったWebGIS作成](https://zenn.dev/asahina820/books/c29592e397a35b) – 毎週の学習に活用できる章を記載  
- **Week 1 & 2 の拡張課題:**  
  - Week 1: 興味のある既存ウェブ地図を評価（Week01.md）  
  - Week 2: 自分の最終プロジェクトのアイデアを提案（Week02.md）  
- **最終プロジェクトへの積み上げ:**  
  毎週の課題を通じて個々のプロジェクトを段階的に拡張し、コース終了時には**機能的**で**情報価値の高い**ウェブ地図を完成させる。  
- **Week 9でのデザイン強化:**  
  ビジュアル面に配慮した地図づくりを学ぶことで、ユーザーエクスペリエンスを高める。  
- **デザインとインタラクティブ性の統合:**  
  デザインと使いやすさが、どのように影響力のあるウェブ地図を生み出すかを段階的に学習する。  