# UF LipSync - 完全ガイド

BlendShape対応モデル用のリップシンクアニメーション生成Unityパッケージです。UFDataファイルから日本語歌詞のタイミング情報を読み取り、BlendShapeを使用したリップシンクアニメーションを自動生成します。

## 📋 動作環境

- Unity 2020.3 LTS以上
- BlendShape対応モデル（VRM、VRChat、MMD、Unitychan等）
- Newtonsoft.Json（自動インストール）

## 🚀 インストール

### Unity Package Managerを使用

1. Unity Package Managerを開く
2. 「+」ボタンをクリックし「Add package from git URL」を選択
3. 以下のURLを入力：
   ```
   https://github.com/mikucrossing/uflipsync.git
   ```

### 手動インストール

1. このリポジトリをクローンまたはダウンロード
2. `Packages`フォルダに配置
3. Unity Package Managerで認識されることを確認

## 📖 使用方法

### 基本的な手順

1. **ツールを開く**
   - メニューバーから `UFLipsync > リップシンク生成` を選択

2. **UFDataファイルを準備**
   - Utaformatixで歌声合成ファイルを`.ufdata`形式に変換
   - UnityプロジェクトにTextAssetとして配置

3. **モデルを設定**
   - BlendShape対応モデルをシーンまたはプロジェクトに配置
   - BlendShapeが自動検出されない場合は手動設定

4. **アニメーション生成**
   - 「リップシンクアニメーションを生成」ボタンをクリック
   - 生成されたAnimationClipを使用

### サポートされるBlendShape

#### 自動検出対象のBlendShapeパターン
- **あ行**: A, a, Fcl_MTH_A, blendShape.A, vrc.v_aa, MTH_A, mouth_a, A02, Mouth_A, あ, Viseme_AA
- **い行**: I, i, Fcl_MTH_I, blendShape.I, vrc.v_ih, MTH_I, mouth_i, I02, Mouth_I, い, Viseme_IH
- **う行**: U, u, Fcl_MTH_U, blendShape.U, vrc.v_ou, MTH_U, mouth_u, U02, Mouth_U, う, Viseme_OU
- **え行**: E, e, Fcl_MTH_E, blendShape.E, vrc.v_e, MTH_E, mouth_e, E02, Mouth_E, え, Viseme_EH
- **お行**: O, o, Fcl_MTH_O, blendShape.O, vrc.v_oh, MTH_O, mouth_o, O02, Mouth_O, お, Viseme_OH

#### 音素判定ルール
- **あ行（A）**: あ、か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、や、ら、わ
- **い行（I）**: い、き、ぎ、し、じ、ち、ぢ、に、ひ、び、ぴ、み、り
- **う行（U）**: う、く、ぐ、す、ず、つ、づ、ぬ、ふ、ぶ、ぷ、む、ゆ、る
- **え行（E）**: え、け、げ、せ、ぜ、て、で、ね、へ、べ、ぺ、め、れ
- **お行（O）**: お、こ、ご、そ、ぞ、と、ど、の、ほ、ぼ、ぽ、も、よ、ろ、を

## 🛠️ UFDataファイルの作成

### Utaformatixを使用した変換

1. **Utaformatix 3にアクセス**
   - https://sdercolin.github.io/utaformatix3/

2. **対応入力形式**
   - VOCALOID: `.vsqx`, `.vpr`, `.vsq`, `.mid`
   - UTAU: `.ust`, `.ustx`
   - CeVIO: `.ccs`
   - SynthV: `.svp`, `.s5p`
   - DeepVocal: `.dv`
   - Piapro Studio NT: `.ppsf`
   - その他: `.mid` (標準), `.xml`/`.musicxml` (MusicXML)

3. **変換手順**
   - プロジェクトファイルをインポート
   - 出力形式で「UfData」を選択
   - エクスポートして`.ufdata`ファイルを取得
   - UnityプロジェクトのTextAssetとして配置

## ⚙️ 詳細設定

### 基本設定
- **UFDataファイル**: 処理対象のUFDataファイル（TextAsset）
- **対象トラック**: 複数トラックがある場合のトラック選択

### モデル設定
- **モデル**: 対象のBlendShape対応モデル（PrefabまたはGameObject）
- **BlendShape検出**: 自動検出または手動設定
- **設定の永続化**: 手動設定は自動保存される

### 詳細設定
- **出力パス**: アニメーション保存先（デフォルト: `Assets/Animations`）
- **最大フェード時間**: 0.1〜1.0秒（デフォルト: 0.3秒）
- **フェード時間比率**: 0.1〜0.5（デフォルト: 0.3）

## 📁 プロジェクト構成

```
Runtime/
├── Models/              # データモデル
│   ├── UFDataModel.cs   # UFDataの構造定義
│   └── LyricTimingModel.cs # 歌詞タイミング情報
├── Services/            # サービス層
│   ├── UFDataLoader.cs  # UFDataファイル読み込み
│   └── UtaformatixTimingCalculator.cs # タイミング計算
└── Utils/               # ユーティリティ
    ├── UFConstants.cs   # 定数定義
    └── LipShapeUtil.cs  # 音素変換ユーティリティ

Editor/
├── LipSyncAnimationGeneratorWindow.cs # メインウィンドウ
├── BasicSettingsUI.cs  # 基本設定UI
├── VRMSettingsUI.cs    # VRM設定UI
├── AdvancedSettingsUI.cs # 詳細設定UI
├── LipSyncAnimationService.cs # アニメーション生成サービス
├── VRMBlendShapeDetector.cs # BlendShape自動検出
├── ManualBlendShapeSelector.cs # 手動BlendShape設定
├── UFDataImporter.cs   # UFDataインポート処理
└── LipSyncGeneratorSettings.cs # 設定の永続化
```

## 🎮 使用例

### Animatorでの利用
```csharp
// Animation Componentでの再生
GetComponent<Animation>().Play("LipSync_song");

// Animatorでの再生
GetComponent<Animator>().Play("LipSyncState");
```

### Timelineでの利用
1. TimelineにAnimation Trackを追加
2. 生成されたAnimationClipをドラッグ&ドロップ
3. 音声トラックと同期させて配置

### スクリプトでの使用
```csharp
// Animation Componentでの再生
GetComponent<Animation>().Play("LipSync_song");

// Animatorでの再生
GetComponent<Animator>().Play("LipSyncState");
```

## 🔧 トラブルシューティング

### よくある問題と解決方法

**「UFDataファイルを選択してください」エラー**
- UFDataファイルが選択されていません
- TextAssetとして正しくインポートされているか確認

**「選択されたUFDataファイルにトラックが見つかりません」エラー**
- UFDataファイルにトラック情報が含まれていません
- Utaformatixでの変換時にトラック情報が正しく保存されているか確認

**「モデルを選択してください」エラー**
- BlendShape対応モデルが選択されていません
- プロジェクト内のモデルを選択してください

**「UFDataの読み込みに失敗しました」エラー**
- UFDataファイルの形式が正しくない可能性があります
- ファイルが破損していないか確認してください

**BlendShapeが動作しない**
- モデルの選択が正しいか確認
- BlendShapeの自動検出が成功しているか確認
- 手動設定の場合、BlendShape名が正確に入力されているか確認
- SkinnedMeshRendererコンポーネントがあるか確認

**アニメーションが再生されない**
- AnimationClipが正しく生成されているか確認
- AnimatorControllerの設定を確認
- 対象オブジェクトがアクティブか確認

## 🚀 高度な使用例

### 複数キャラクターでの使用
1. キャラクターごとに対象フェイスパスを変更
2. 同じUFDataから複数のアニメーションを生成
3. 各キャラクターに対応するアニメーションを適用

### 複数トラックの活用
1. **ハーモニー**: 異なるトラックからそれぞれリップシンクアニメーションを生成
2. **キャラクター別**: トラックごとに異なるキャラクターのアニメーションを作成
3. **パート分け**: 主旋律とコーラスパートを分けて処理

### カスタムBlendShape名での使用
- **自動検出**: VRMモデルから自動的にBlendShapeを検出
- **手動設定**: 自動検出が失敗した場合、手動で音素とBlendShapeの対応を設定
- **設定保存**: 手動設定した内容は自動的に保存される

## 📄 生成されるアニメーション

### ファイル名形式
`LipSync_[モデル名]_[UFDataファイル名]_Track[番号].anim`

例：
- モデル: `Hoge`、UFData: `song.ufdata`、トラック0 → `LipSync_Hoge_song_Track0.anim`
- モデル: `Avatar`、UFData: `test.ufdata`、トラック1 → `LipSync_Avatar_test_Track1.anim`

### アニメーションの特徴
- **フェードイン**: 音素開始前からなめらかに口の形を変化
- **維持**: 音素継続中は対応する口の形を保持
- **フェードアウト**: 音素終了時になめらかに元に戻る
- **重複処理**: 連続する音素間の重複は自動的に調整

## 🔍 音素判定の仕組み

### 優先順位
1. **Phoneme情報**: UFDataにPhoneme情報がある場合は優先使用
2. **歌詞解析**: Phonemeがない場合は歌詞の最後の文字から判定

### 設定の永続化について

このツールでは、すべての設定が自動的に保存されます：

- **設定ファイル**: `Assets/Editor/LipSync/LipSyncGeneratorSettings.asset`
- **自動保存**: 設定変更時に即座に保存
- **エディタ再起動**: 前回の設定が復元される

設定をリセットしたい場合は、設定ファイルを削除すると次回起動時にデフォルト値で再作成されます。

## 💡 開発者向け情報

各クラスの詳細なAPI仕様や使用方法については、ソースコード内の`<summary>`タグに日本語で記載されています。IntelliSenseやIDEのドキュメント表示機能をご活用ください。

## 📄 ライセンス

このプロジェクトは[MIT License](../LICENSE)の下で公開されています。

### 使用ライブラリ・データ形式

このプロジェクトでは以下のオープンソースプロジェクトのデータ形式仕様を参考にしています：

- **utaformatix-data** by [sdercolin](https://github.com/sdercolin)
  - UFDataファイル形式の仕様
  - ライセンス: Apache License 2.0
  - リポジトリ: https://github.com/sdercolin/utaformatix-data
  - 本プロジェクトではUnity向けに独自実装しており、元のコードを直接使用していません

## 🤝 貢献

バグ報告や機能要求はIssuesにお寄せください。プルリクエストも歓迎します。

## 📞 サポート

- GitHub Issues: バグ報告・機能要求
- 開発者: [minarin0179](https://github.com/minarin0179)

