# SHAKE ROOM

**[English](#english) | [日本語](#japanese)**

<a id="english"></a>

## English

SHAKE ROOM is a single-file, in-browser 3D simulator that drives a virtual room on a shake table with **real strong-motion records** and shows how furniture and small items slide, rock, overturn and fall.

> ⚠️ This tool is for **education and disaster awareness** only. It must not be used to evaluate anti-tipping measures, design buildings or equipment, predict damage, or support legal, insurance or regulatory decisions. See [Disclaimer](#disclaimer).

### Features

- **Strong-motion input**
  - NIED **K-NET / KiK-net CSV** (surface or borehole channels for KiK-net) and the **JMA strong-motion format** (Shift-JIS supported).
  - Multiple files can be loaded and switched.
  - A built-in synthetic motion is available for testing without data.
- **Waveform processing**
  - NIED records: whole-record mean removal, then a Saito (1978) recursive Butterworth high-pass filter (Fl = 0.1 Hz, Fs = 0.05 Hz, Ap = 0.1, As = 10), then trapezoidal integration.
  - JMA records: the recursive integration filters published by JMA (velocity: 3rd-order Butterworth low-cut at 5 s; displacement: reproduction of the JMA mechanical 1× strong-motion seismograph, T0 = 6 s, h = 0.55).
  - Derived values: PGA, PGV, PGD, JMA instrumental seismic intensity, and the 5 %-damped velocity response spectrum (Nigam–Jennings).
  - The shaking onset is detected automatically (STA/LTA + AIC).
- **Station information**: names, prefectures, elevations and instrument types from the NIED station list (embedded; you can replace it by dropping a newer list CSV).
- **Rigid-body physics engine** (written from scratch):
  - Penalty contacts with Coulomb friction using static and kinetic coefficients.
  - Anti-tipping brackets and anchors that break at a set load.
  - Pendant lamps and pipe-hung fluorescent fixtures.
  - Cars on soft tyres and suspension.
  - Stabilisation for light objects pinched between heavy ones.
- **Room presets**
  - Living room, bedroom, dining kitchen, office.
  - Japanese convenience store (about 240 individual goods).
  - School classroom, hospital ward.
- **Building mode**: the room is shaken by the estimated motion of any floor of an RC, steel or base-isolated building. The building response is a nonlinear shear-building model (bilinear hysteresis, Ai-distributed storey strength, Newmark method).
- **Layout editing**
  - A 2D floor-plan editor: move, rotate, snap to a wall, add and delete items, and choose what sits on each piece.
  - Click any object in the 3D view to delete it.
  - Optional mode that simulates every book on a bookshelf individually.
- **Precompute mode**: heavy scenes are computed first, then played back and scrubbed in real time.
- **Video export**: MP4 (H.264, or VP9/AV1 in MP4) via WebCodecs, falling back to WebM. Time, waveform and status overlays are optional.
- **Bilingual UI** (日本語 / English) with built-in help covering the disclaimer, model limitations, usage and FAQ.

### Quick start

1. Open `shake_room.html` in a recent desktop browser (Chrome, Edge, Firefox or Safari). No installation or server is needed; for GitHub Pages, rename it to `index.html`.
2. Drop one or more record CSV files onto the **Strong-motion record** box, or click **Synthetic**.
3. Pick a **Room** preset and press **▶ Play**.

### Getting data

| Source | Format | Where |
|---|---|---|
| NIED K-NET / KiK-net | K-NET CSV | NIED strong-motion network website (registration required) |
| Japan Meteorological Agency | JMA strong-motion format (CSV) | JMA "strong-motion observation data" pages for major earthquakes |

Follow each provider's terms of use, and credit the provider when you publish results.

### Controls

| Action | Input |
|---|---|
| Orbit / pan / zoom | Left-drag / right-drag / mouse wheel |
| Play / pause | Space |
| Delete the selected object | Click it, then press Delete |
| Deselect / close dialogs | Esc |
| Help | `?` button, `?` key or F1 |
| Floor plan | R: rotate 90° · E: rotate 15° · W: snap to wall · arrow keys: nudge · Del: delete |

### Physical model and limitations

- Furniture is modelled as **rigid bodies**: no deformation or breakage, drawers and doors do not open, and contents are fixed inside (except in the per-book option).
- Collision shapes are **unions of boxes**, so cylindrical items such as cans and bottles do not roll.
- Contacts are **penalty springs**, so objects overlap by millimetres to centimetres. Bodies whose contacts become too stiff for the time step (1/1800 s) are slowed for stability (selective mass scaling).
- Friction coefficients are typical values; real ones vary widely. Casters are approximated by low friction.
- Long-period motion removed by the filters (beyond about 10 s for NIED and about 5 s for JMA) is not reproduced. Rotational ground motion, floor tilt and soil–structure interaction are ignored.
- The building model is a rough estimate. Vertical motion is taken equal to the ground, and torsion is ignored.
- Overturning is **chaotic**: treat each run as one possible outcome, not a prediction.

### Technical notes

- Everything lives in one HTML file. Three.js r128 and mp4-muxer are loaded from jsDelivr; fonts come from Google Fonts.
- Records are processed only in the browser and never uploaded.
- Language and help preferences are stored in `localStorage`.

<a id="disclaimer"></a>

### Disclaimer

This software is provided "as is", without warranty of any kind. The author accepts no liability for any damage arising from its use or from its results. This tool is not affiliated with NIED, JMA or any other data provider.

---

<a id="japanese"></a>

## 日本語

SHAKE ROOM は、**実際の強震記録**で振動台上の部屋を揺らし、家具や小物の滑動・ロッキング・転倒・落下を3Dで表示するシミュレータです。HTMLファイル1つでブラウザ上だけで動きます。

> ⚠️ 本ツールは**教育・防災啓発**のためのものです。家具の転倒防止策の効果判定、建築物や設備の設計、被害予測には使わないでください。詳しくは[免責事項](#disclaimer-ja)を参照してください。

### 主な機能

- **強震記録の読込**
  - 防災科研の **K-NET／KiK-net CSV**（KiK-netは地表と地中を選択可）と、**気象庁強震観測フォーマット**（Shift-JIS対応）に対応しています。
  - 複数ファイルを読み込んで切り替えられます。
  - 手元にデータがなくても、合成波で動作を確認できます。
- **波形処理**
  - NIEDの記録：全区間の平均を除去し、斎藤（1978）の漸化式バタワース・ハイパスフィルタ（Fl=0.1Hz, Fs=0.05Hz, Ap=0.1, As=10）をかけて台形積分します。
  - 気象庁の記録：気象庁が公表している積分漸化式を使います（速度＝周期5秒以上をカットする3次バタワース、変位＝機械式1倍強震計 T0=6秒・h=0.55 の再現）。
  - 算出する値：最大加速度・最大速度・最大変位、計測震度、h=5%の速度応答スペクトル（Nigam–Jennings法）。
  - 揺れ始めは STA/LTA と AIC で自動検出します。
- **観測点情報**：NIEDの観測点リストから観測点名・都道府県・標高・強震計機種を表示します。リストは組み込み済みで、新しいリストCSVをドロップすると置き換えられます。
- **独自の剛体物理エンジン**
  - ペナルティ法による接触と、静・動摩擦係数を使うクーロン摩擦。
  - 一定の荷重で破断するL字金具・アンカー。
  - 吊り下げ照明、パイプ2点吊りの蛍光灯。
  - タイヤとサスペンションで揺れる自動車。
  - 重い物に挟まれた軽い物が跳ね飛ばないための安定化処理。
- **室内プリセット**
  - リビング、寝室、ダイニングキッチン、オフィス。
  - コンビニ（商品約240点を個別に計算）。
  - 学校の教室、病院の病室
- **建物モード**：RC造・S造・免震RC造の任意の階の揺れを推定して加振します。建物の応答は弾塑性せん断型多質点モデル（バイリニア型、Ai分布の層せん断耐力、Newmark法）で計算します。
- **配置の編集**
  - 平面図で、移動・回転・壁寄せ・追加・削除と、家具の上に載せる物の選択ができます。
  - 3D画面で物をクリックして削除できます。
  - 本棚の本を1冊ずつ計算するオプションがあります。
- **先に計算してから再生**：重いシーンも、先にまとめて計算してから実時間で再生・シークできます。
- **動画出力**：WebCodecsでMP4（H.264、またはVP9/AV1）を書き出します。非対応のブラウザではWebMになります。時刻・波形・状態の重ね表示を選べます。
- **日本語／英語の切替**と、免責事項・モデルの制約・使い方・よくある質問を載せた内蔵ヘルプ。

### 使い方

1. `shake_room.html` を最新のPC向けブラウザ（Chrome、Edge、Firefox、Safari）で開きます。インストールやサーバーは不要です。GitHub Pagesで公開する場合は `index.html` にリネームしてください。
2. 「強震記録」欄にCSVをドロップします。手元になければ「合成波」を押します。
3. 「室内」でプリセットを選び、**▶ 再生** を押します。

### データの入手先

| 提供元 | 形式 | 入手先 |
|---|---|---|
| 防災科学技術研究所 K-NET／KiK-net | K-NET CSV | 強震観測網のWebサイト（利用登録が必要） |
| 気象庁 | 強震観測フォーマット（CSV） | 気象庁「強震観測データ」（主な地震） |

各提供元の利用条件に従ってください。成果を公表する際は、提供元を明記してください。

### 操作

| 操作 | 入力 |
|---|---|
| 視点の回転／平行移動／ズーム | 左ドラッグ／右ドラッグ／ホイール |
| 再生・一時停止 | Space |
| 選択した物の削除 | クリックして Delete |
| 選択解除・ダイアログを閉じる | Esc |
| ヘルプ | ?ボタン、?キー、F1 |
| 平面図 | R：90°回転、E：15°回転、W：壁に寄せる、矢印キー：微調整、Del：削除 |

### 物理モデルと制約

- 家具は**剛体**として扱います。変形・破損はせず、引出しや扉は開きません。収納物は家具と一体です（本を1冊ずつ計算するオプションを除く）。
- 当たり判定は**直方体の組合せ**です。缶やボトルなど円柱形の物も転がりません。
- 接触は**ペナルティ法**のため、mm〜cm程度のめり込みが生じます。時間刻み（1/1800秒）に対して接触が硬くなりすぎた物体は、安定化のため動きを鈍くしています（選択的質量スケーリング）。
- 摩擦係数は代表値で、実際には大きくばらつきます。キャスターは小さな摩擦係数で近似しています。
- フィルタで除去される長周期成分（NIED方式で約10秒以上、気象庁方式で約5秒以上）は再現しません。回転成分、床の傾き、地盤と建物の相互作用は考慮していません。
- 建物モードは概略の推定です。上下動は地表と同じとし、ねじれは考慮していません。
- 転倒は**カオス的**で、わずかな条件の違いで結果が変わります。1回の結果は「起こり得る一例」として扱ってください。

### 技術メモ

- HTMLファイル1つで完結しています。Three.js r128 と mp4-muxer は jsDelivr から、フォントは Google Fonts から読み込みます。
- 読み込んだ記録はブラウザ内だけで処理し、外部には送信しません。
- 言語とヘルプの表示設定は `localStorage` に保存します。

<a id="disclaimer-ja"></a>

### 免責事項

本ソフトウェアは現状のまま提供され、いかなる保証もありません。本ソフトウェアの使用やその結果によって生じたいかなる損害についても、作者は責任を負いません。本ツールは、防災科学技術研究所・気象庁その他のデータ提供元とは無関係です。
