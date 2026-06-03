
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI文章生成 学習サイト（基礎編・専門編 分離版＋専門補足）</title>
  <style>
    :root {
      --bg1:#f1f5f9; --bg2:#dbeafe; --card:#ffffff; --text:#0f172a; --muted:#475569;
      --line:#cbd5e1; --blue:#2563eb; --blue2:#3b82f6; --soft:#f8fafc; --shadow:0 20px 40px rgba(15,23,42,.10);
      --radius:24px; --green:#10b981; --amber:#f59e0b; --gold-bg:#fffbeb; --gold-line:#fde68a; --gold-text:#92400e;
    }
    *{box-sizing:border-box}
    body{margin:0;min-height:100vh;font-family:Arial,'Hiragino Kaku Gothic ProN',Meiryo,sans-serif;color:var(--text);background:linear-gradient(135deg,var(--bg1),var(--bg2))}
    .page{max-width:1200px;margin:0 auto;padding:24px}
    .header{text-align:center;margin-bottom:24px}
    .header h1{margin:0;font-size:44px;line-height:1.15}.header p{margin:10px 0 0;color:var(--muted);font-size:17px}
    .tabbar,.subtabbar{display:flex;justify-content:center;gap:12px;flex-wrap:wrap}
    .tabbar{margin-bottom:24px}.subtabbar{margin:0 0 18px}
    .btn{appearance:none;border:1px solid var(--line);background:#fff;color:var(--text);padding:11px 16px;border-radius:14px;cursor:pointer;font-size:14px;font-weight:700}
    .btn.primary{background:var(--blue);color:#fff;border-color:var(--blue)}
    .card{background:var(--card);border-radius:var(--radius);box-shadow:var(--shadow);padding:28px;margin-bottom:24px}
    .section-title{font-size:28px;font-weight:700;margin:0 0 16px}.sub-title{font-size:14px;color:var(--muted);margin-bottom:14px;line-height:1.85}
    .grid-2{display:grid;grid-template-columns:1.02fr .98fr;gap:24px}.grid-half{display:grid;grid-template-columns:1fr 1fr;gap:24px}
    select{width:100%;border:1px solid var(--line);border-radius:14px;padding:14px 16px;font-size:16px;background:#fff}
    .soft-block{background:var(--soft);border:1px solid #e2e8f0;border-radius:18px;padding:16px;margin-bottom:12px}
    .soft-title{font-size:13px;font-weight:700;color:#334155;margin-bottom:6px}.soft-body{font-size:14px;color:var(--muted);line-height:1.95}
    .result{min-height:120px;font-size:24px;line-height:1.9;color:#334155;background:#f8fafc;border:1px dashed #cbd5e1;border-radius:16px;padding:18px}.pending{color:#b45309;margin-left:6px}
    .cand-item{margin-bottom:12px}.cand-row{display:flex;justify-content:space-between;gap:12px;font-size:14px;margin-bottom:4px}.bar-outer{height:10px;border-radius:999px;background:#e2e8f0;overflow:hidden}.bar-inner{height:100%;border-radius:999px;background:var(--blue2)}.tag{display:inline-block;background:#eff6ff;color:var(--blue);border:1px solid #bfdbfe;border-radius:999px;padding:4px 10px;font-size:12px;font-weight:700;margin-left:8px}
    .history-item{color:var(--muted);font-size:14px;margin-bottom:6px}.log{background:#f8fafc;border-radius:16px;padding:14px;margin-bottom:10px}.log-title{font-weight:700;margin-bottom:6px}.status{margin-top:14px;color:var(--muted)}
    .progress-wrap{display:flex;gap:8px;margin-bottom:10px}.progress-seg{flex:1;height:10px;border-radius:999px;background:#e5e7eb}.progress-seg.active{background:var(--green)}.progress-seg.current{background:#a7f3d0}
    .explain{background:#eff6ff;border:1px solid #bfdbfe;color:#1e3a8a;border-radius:16px;padding:14px;line-height:1.8;margin-top:14px}
    .reason-box{background:var(--gold-bg);border:1px solid var(--gold-line);color:var(--gold-text);border-radius:16px;padding:14px;line-height:1.8}
    .scroll-panel{max-height:360px;overflow-y:auto}.reason-entry{padding:0 0 12px;margin-bottom:12px;border-bottom:1px dashed #fcd34d}.reason-entry:last-child{border-bottom:none;margin-bottom:0;padding-bottom:0}.reason-entry-title{font-weight:700;margin-bottom:6px}.reason-list{margin:0;padding-left:20px}
    .theory-grid{display:grid;gap:16px}.theory-box{background:#f8fafc;border:1px solid #e2e8f0;border-radius:18px;padding:16px}.theory-box h3{margin:0 0 8px;font-size:20px}
    .detail-box{background:#fff;border:1px solid #dbeafe;border-radius:18px;padding:14px 16px}.detail-box + .detail-box{margin-top:12px}.detail-box summary{cursor:pointer;font-weight:700;color:#1d4ed8}.detail-body{margin-top:10px;color:var(--muted);line-height:2.02}
    .summary-box{background:#eff6ff;border:1px solid #bfdbfe;border-radius:18px;padding:16px}
    .summary-box h3{margin:0 0 8px;font-size:18px;color:#1d4ed8}
    .term{position:relative;display:inline-block;color:#1d4ed8;border-bottom:2px dotted #93c5fd;cursor:help;font-weight:700}
    .term:hover::after,.term:focus-visible::after{content:attr(data-note);position:absolute;left:50%;transform:translateX(-50%);bottom:calc(100% + 14px);width:min(380px,78vw);background:#fff;color:#334155;border:1px solid #dbeafe;border-radius:18px;box-shadow:0 16px 30px rgba(15,23,42,.12);padding:16px 18px;line-height:1.9;font-weight:500;font-size:14px;z-index:30;white-space:normal;text-align:left}
    .term:hover::before,.term:focus-visible::before{content:"";position:absolute;left:50%;transform:translateX(-50%);bottom:calc(100% + 6px);border-width:8px;border-style:solid;border-color:#fff transparent transparent transparent;z-index:31;filter:drop-shadow(0 -1px 0 #dbeafe)}
    code.inline{background:#eef2ff;border:1px solid #c7d2fe;padding:2px 6px;border-radius:8px;font-family:inherit}
    ul{margin:0;padding-left:20px}
    @media (max-width:900px){.grid-2,.grid-half{grid-template-columns:1fr}.header h1{font-size:34px}}
  </style>
</head>
<body>
<div class="page">
  <header class="header">
    <h1>AI文章生成 学習サイト</h1>
    <p>AIが「次の表現」ではなく、「次の単語」に近い単位を一つずつ選んで文を作る流れを体験できます。</p>
  </header>

  <div class="tabbar">
    <button id="tab-demo" class="btn primary">生成体験</button>
    <button id="tab-theory" class="btn">原理解説</button>
  </div>

  <section id="theory-panel" style="display:none;">
    <div class="card">
      <h2 class="section-title">原理解説</h2>
      <div class="subtabbar">
        <button id="subtab-basic" class="btn primary">基礎編</button>
        <button id="subtab-advanced" class="btn">専門編</button>
      </div>

      <section id="basic-panel">
        <div class="summary-box" style="margin-bottom:16px;">
          <h3>基礎編の読み方</h3>
          <div class="soft-body">まずは「AIは一気に文を書いているのではなく、次の単語を少しずつ選んでいる」という大きな見方をつかむのがおすすめです。そのうえで、なぜ単語候補に差が出るのか、なぜ毎回まったく同じ結果にはならないのかを見ると、生成画面の動きと説明が結びつきやすくなります。</div>
        </div>
        <div class="theory-grid">
          <div class="theory-box">
            <h3>1. 文章は一気に決まるのではなく、少しずつ決まる</h3>
            <div class="soft-body">AIは最初から完成した一文を丸ごと持っているわけではありません。まず今ある流れを見て、次に来そうな言葉をいくつか比べ、その中からひとつを選びます。選んだ言葉を文に足したら、またその先を考えます。このくり返しで文章が少しずつ伸びていきます。人が文章を書くときに、頭の中で「次はこう続けようかな」と考えながら書く感じに少し似ていますが、AIはそれを大量の言葉の学習結果にもとづいて機械的に行っています。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">この考え方を押さえると、AIは「完成文を倉庫から取り出している」のではなく、「その場で次の一歩を決めている」と見たほうが近いと分かります。つまり、完成した文章は最初から決まっているものではなく、途中の選択の積み重ねとして形づくられていきます。</div></details>
          </div>
          <div class="theory-box">
            <h3>2. AIが見ているのは「今までの流れ」</h3>
            <div class="soft-body">AIは直前の一語だけではなく、今までに出てきた言葉の流れ全体を手がかりにします。たとえば <code class="inline">春は</code> と始まっていれば、季節や自然に関係する言葉が出やすくなりますし、<code class="inline">野球は</code> と始まっていれば、試合や応援に関係する言葉が出やすくなります。つまり、同じ「次の一語」を選ぶ場面でも、前に何が書かれていたかによって答えが変わります。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">ここで大事なのは、AIが一語だけ見て機械的に反応しているわけではないという点です。前に出た言葉の積み重ねが、次の候補の出方を変えます。だから同じ単語でも、前の流れが違うとそのあとに続きやすい言葉も変わります。</div></details>
          </div>
          <div class="theory-box">
            <h3>3. 次に来やすさは数字で表せる</h3>
            <div class="soft-body">画面に出る確率は、「今の流れのあとに、その単語がどれくらい自然そうか」を表しています。実際のAIでも、<span class="term" tabindex="0" data-note="候補それぞれに対して、どれくらい出やすいかを数字で表したものです。全部を合わせると100%になります。">確率分布</span> をもとに次の言葉を決める考え方が使われます。つまり、AIは「ひとつの正解だけ」を持っているのではなく、複数の候補を出し、その出やすさに差をつけていると考えられます。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">この見方が分かると、AIの出力は「絶対に一つの答えしかない」ものではなく、「より出やすいものが上に来る」ものだと理解しやすくなります。だから、少し低い候補が選ばれた場合でも、不自然というより「別の続き方が出た」と見るほうが実態に近いです。</div></details>
          </div>
          <div class="theory-box">
            <h3>4. なぜその単語が選ばれたのか</h3>
            <div class="soft-body">単語が選ばれる理由は、大きく分けると「テーマとの合いやすさ」「前の言葉とのつながり」「その先の文の作りやすさ」です。この教材では「なぜこの単語？」の欄で、その理由を順番に見返せるようにしています。つまり、AIは目の前の一語だけを機械的に当てているのではなく、前後の流れの中で「いちばん無理がない続き方」を探している、と考えるとイメージしやすいです。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">ここで見てほしいのは、テーマと文のつながりの両方が効いていることです。テーマに合っていても前後の流れに合わなければ選ばれにくく、逆に文としてつながってもテーマから外れすぎると選ばれにくくなります。このバランス感覚が、AIの文章の自然さを支えています。</div></details>
          </div>
          <div class="theory-box">
            <h3>5. 実際のAIとの違い</h3>
            <div class="soft-body">実際の言語モデルは、ここで見せているよりさらに細かい「<span class="term" tabindex="0" data-note="AIが文章を扱うときに使う、小さな区切りです。単語そのもののときもあれば、単語の一部や記号のときもあります。">トークン</span>」という単位で処理することが多いです。このページでは理解しやすいように、意味のかたまりに近い単位を「次の単語」として見せています。考え方は本物に近いですが、表示は学習しやすい形にやさしく整えています。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">つまり、この教材は「本物そのもの」ではなく、「本物の考え方を分かりやすく見える形にしたもの」です。こう考えると、画面での単語の見え方と、実際のAIの内部処理の違いを区別しやすくなります。</div></details>
          </div>
        </div>
      </section>

      <section id="advanced-panel" style="display:none;">
        <div class="summary-box" style="margin-bottom:16px;">
          <h3>専門編の入り口</h3>
          <div class="soft-body">ここから先は、AIを少し「内部の構造」や「学習の仕方」から見る内容です。言葉を数値として扱うこと、学習時に何が更新されるのか、現在の大規模言語モデルの代表的な構造が何か、といった点をまとめています。</div>
        </div>
        <div class="theory-grid">
          <div class="theory-box">
            <h3>1. 言葉はどうやって数値になるのか</h3>
            <div class="soft-body">AIの内部では、言葉はまず数値に置きかえられます。その代表的な考え方が <span class="term" tabindex="0" data-note="言葉を多次元の数値として表したものです。意味や使われ方が近い言葉は、近い位置に表れやすくなります。">埋め込み表現</span> です。たとえば「春」「桜」「花」のように一緒に使われやすい言葉は、内部の数値表現でも近い関係になりやすいです。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">埋め込み表現は、言葉をただ番号に変えるだけではありません。意味や役割の近い言葉が、内部の数値空間でも近く置かれやすくなるため、AIは似た表現どうしの関係を扱いやすくなります。これによって、表面上の文字が違っても、近い意味をある程度まとめて考えられるようになります。</div></details>
          </div>
          <div class="theory-box">
            <h3>2. 学習では何をしているのか</h3>
            <div class="soft-body">AIの学習では、大量の文章を読み込みながら「次に来る言葉を当てる」練習を何度も繰り返します。そのとき、予想が外れたぶんだけ内部の <span class="term" tabindex="0" data-note="モデルの中に保存されている調整用の数値です。学習によって少しずつ更新されます。">パラメータ</span> を少し修正して、次はうまく当てられるようにします。この「どれくらい外れたか」を数値で表したものが <span class="term" tabindex="0" data-note="予想がどれくらい外れていたかを表す数字です。この値を小さくする方向で学習が進みます。">損失</span> です。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">ここで重要なのは、学習が「答えをそのまま保存すること」ではなく、「予想のしかたを少しずつ調整すること」だという点です。予想が外れたときに、内部の数値をどの方向へどれくらい動かすかを重ねることで、だんだん自然な続きを出しやすくなります。</div></details>
          </div>
          <div class="theory-box">
            <h3>3. Transformer とは何か</h3>
            <div class="soft-body">現在の多くの言語モデルは、<span class="term" tabindex="0" data-note="言葉のつながりを広く見ながら計算するための代表的なモデル構造です。大規模言語モデルの土台によく使われます。">Transformer</span> という構造をもとにしています。この構造の特徴は、文の中のどの位置の言葉にも比較的広く注目できることです。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">Transformer が強いのは、離れた位置の言葉どうしの関係も比較的扱いやすいことです。文の前半に出てきた話題が後半の表現にどう影響するかを見やすくなるため、長めの説明や会話にも対応しやすくなります。</div></details>
          </div>
          <div class="theory-box">
            <h3>4. 事前学習と追加の調整</h3>
            <div class="soft-body">大きな言語モデルは、まず非常に多くの文章を読んで、一般的な言葉のつながりを学ぶ段階があります。これを <span class="term" tabindex="0" data-note="最初に大量のデータで広く学習する段階です。言葉の一般的なつながりを学びます。">事前学習</span> と考えると分かりやすいです。そのあと、会話らしく答える、安全に答える、といった方向づけを行うことがあります。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">大まかにいうと、事前学習は「広く覚える段階」、その後の調整は「どう振る舞うかを整える段階」です。この二段階を分けて考えると、なぜ同じ言語モデルでも用途によって応答の傾向が違うのかを理解しやすくなります。</div></details>
          </div>
          <div class="theory-box">
            <h3>5. モデルの中では何が保存されているのか</h3>
            <div class="soft-body">AIモデルの中には、文章そのものが本のように保存されているわけではありません。保存されているのは主に膨大な数の数値で、これらは「どのような流れのときに、どのような表現が出やすいか」という傾向を表しています。</div>
            <details class="detail-box"><summary>補足説明</summary><div class="detail-body">この点は、AIを単なる検索と区別するときに重要です。検索は保存された情報を見つけることが中心ですが、言語モデルは内部の数値からその場で次の出力を作ります。だから、自然な文章は作れても、事実確認は別に必要になることがあります。</div></details>
          </div>
        </div>
      </section>
    </div>
  </section>

  <section id="demo-panel">
    <div class="grid-2">
      <section class="card">
        <h2 class="section-title">テーマ選択</h2>
        <div class="sub-title">あらかじめ用意したテーマを選ぶと、そのテーマに合った単語候補が順番に出てきます。</div>
        <select id="theme-select"></select>
        <button id="generate-btn" class="btn primary" style="margin-top:16px;">AI生成開始</button>
        <div class="status">状態: <span id="ai-mood">待機中</span></div>
      </section>
      <section class="card">
        <h2 class="section-title">今、AIが見ていること</h2>
        <div class="soft-block"><div class="soft-title">選択テーマ</div><div class="soft-body" id="view-topic"></div></div>
        <div class="soft-block"><div class="soft-title">話題カテゴリ</div><div class="soft-body" id="view-category"></div></div>
        <div class="soft-block"><div class="soft-title">想定キーワード</div><div class="soft-body" id="view-keywords"></div></div>
        <div class="soft-block"><div class="soft-title">進行状況</div><div id="progress-area" class="soft-body"></div></div>
      </section>
    </div>
    <div class="grid-half">
      <section class="card">
        <h2 class="section-title">生成中の文章</h2>
        <div class="sub-title">選ばれた単語が一つずつつながっていく様子を確認できます。</div>
        <div class="result" id="current-sentence">まだ生成されていません<span class="pending"></span></div>
        <div class="explain" id="current-context">ここには、今の流れからどんな単語が候補になるかの見方が表示されます。</div>
      </section>
      <section class="card">
        <h2 class="section-title">なぜこの単語？</h2>
        <div class="sub-title">これまで選ばれた単語の理由をすべて表示します。下にスクロールして見返せます。</div>
        <div class="reason-box scroll-panel" id="why-word-history">生成を始めると、ここに選ばれた理由が順番に表示されます。</div>
      </section>
    </div>
    <section class="card">
      <h2 class="section-title">次の単語候補と確率</h2>
      <div class="sub-title">そのときの流れから、次に続きそうな単語候補を比べます。</div>
      <div id="candidates-area" class="soft-body">生成を開始すると、ここに候補と確率が表示されます。</div>
    </section>
    <div class="grid-2">
      <section class="card"><h2 class="section-title">最終生成結果</h2><div class="result" id="final-sentence">ここに完成した文が表示されます</div><div id="selected-words" class="soft-body">まだありません</div></section>
      <section class="card"><h2 class="section-title">思考プロセス</h2><div id="thinking-area" class="soft-body">生成後に思考プロセスが表示されます。</div></section>
    </div>
    <section class="card"><h2 class="section-title">履歴</h2><div id="history-area" class="soft-body">生成履歴はまだありません。</div></section>
  </section>
</div>
<script>
const topics={spring:{label:'春',category:'季節',keywords:['春','桜','暖かさ','新生活'],templates:[['春は','暖かい','風が','吹き、','桜が','美しく','咲く','季節です。'],['春になると','新生活が','始まり、','街の','雰囲気も','明るく','なります。'],['やわらかな','空気や','花の','変化から、','春らしさが','感じられます。']]},summer:{label:'夏',category:'季節',keywords:['夏','青空','海','暑さ'],templates:[['夏は','青空が','広がり、','海や','空の','印象が','強くなる','季節です。'],['入道雲を','見ると、','夏らしさを','強く','感じます。'],['暑さの','中にも、','元気な','雰囲気が','あるのが','夏の','特徴です。']]},autumn:{label:'秋',category:'季節',keywords:['秋','紅葉','実り','涼しさ'],templates:[['秋は','涼しさを','感じながら、','紅葉の','変化を','楽しめる','季節です。'],['実りの','季節として、','多くの','自然の','変化が','見られます。'],['澄んだ','空気や','月の','印象も、','秋らしさを','強めます。']]},winter:{label:'冬',category:'季節',keywords:['冬','雪','寒さ','温かさ'],templates:[['冬は','寒さが','厳しい','一方で、','温かさの','大切さを','感じる','季節です。'],['雪が','降ると、','景色が','一気に','冬らしく','なります。'],['静かで','引き締まった','空気も、','冬ならではの','特徴です。']]},baseball:{label:'野球',category:'スポーツ',keywords:['野球','試合','ホームラン','応援'],templates:[['野球は','最後まで','勝敗が','分からない','ところが','魅力です。'],['ホームランが','出ると、','球場全体が','大きく','盛り上がります。'],['応援の','熱気が','試合の','面白さを','さらに','高めます。']]},soccer:{label:'サッカー',category:'スポーツ',keywords:['サッカー','ゴール','連携','スピード'],templates:[['サッカーは','スピード感と','連携の','良さが','魅力です。'],['ゴールが','決まった','瞬間の','盛り上がりは','とても','大きいです。'],['パスの','つながりを','見ると、','チームの','力が','よく','分かります。']]},basketball:{label:'バスケットボール',category:'スポーツ',keywords:['バスケットボール','速さ','得点','連携'],templates:[['バスケットボールは','攻守の','切り替えが','速く、','展開の','変化が','大きい','スポーツです。'],['得点が','短い','時間で','動くので、','最後まで','目が','離せません。']]},school:{label:'学校',category:'学校・学習',keywords:['学校','授業','友達','学び'],templates:[['学校では','勉強だけでなく、','人との','関わり方も','学べます。'],['友達との','時間が、','学校生活を','より','豊かに','します。'],['毎日の','授業の','積み重ねが、','将来に','つながって','いきます。']]},research:{label:'自由研究',category:'学校・学習',keywords:['自由研究','観察','発見','まとめ'],templates:[['自由研究では、','観察した','ことを','記録して','まとめる','力が','育ちます。'],['身近な','疑問を','調べると、','新しい','発見に','つながります。']]},library:{label:'図書館',category:'学校・学習',keywords:['図書館','本','学び','調べる'],templates:[['図書館は、','本を','通して','新しい','知識に','出会える','場所です。'],['静かな','空間で','調べものが','できるのが','良さです。']]},space:{label:'宇宙',category:'科学・宇宙',keywords:['宇宙','星','しくみ','広がり'],templates:[['宇宙は','とても','広く、','星や','惑星の','動きを','知ると','興味が','深まります。'],['ロケットや','惑星の','しくみを','知ると、','宇宙の','見え方が','変わります。']]},dinosaur:{label:'恐竜',category:'科学・宇宙',keywords:['恐竜','化石','特徴','発見'],templates:[['恐竜の','研究では、','化石から','昔の','生き物の','特徴を','考えます。'],['発見の','面白さが、','想像を','広げて','くれます。']]},robot:{label:'ロボット',category:'科学・宇宙',keywords:['ロボット','動き','制御','技術'],templates:[['ロボットは','技術の','工夫によって、','さまざまな','動きを','実現しています。'],['制御の','仕組みを','知ると、','面白さが','より','分かります。']]},computer:{label:'コンピューター',category:'科学・技術',keywords:['コンピューター','計算','情報','しくみ'],templates:[['コンピューターは','情報を','処理する','仕組みで、','日常の','多くの','場面を','支えています。'],['計算だけでなく、','文字や','画像を','扱える','ところも','特徴です。']]},internet:{label:'インターネット',category:'科学・技術',keywords:['インターネット','通信','情報','つながり'],templates:[['インターネットは','世界中の','情報を','つなぐ','大きな','仕組みです。'],['便利さの','一方で、','情報の','確かさを','見分ける','ことも','大切です。']]},lunch:{label:'給食',category:'食べ物',keywords:['給食','おいしさ','栄養','楽しさ'],templates:[['給食には','おいしさだけでなく、','栄養の','バランスも','大切に','されています。'],['毎日の','食事には、','体を','支える','役割が','あります。']]},bento:{label:'お弁当',category:'食べ物',keywords:['お弁当','彩り','工夫','栄養'],templates:[['お弁当には','彩りや','栄養の','工夫が','たくさん','あります。'],['小さな','箱の中に、','いろいろな','良さが','詰まっています。']]},bread:{label:'パン',category:'食べ物',keywords:['パン','香り','食感','種類'],templates:[['パンには','香りや','食感の','違いが','あり、','種類ごとの','面白さが','あります。'],['焼きたての','香りは、','食べる','楽しさを','強めます。']]},dog:{label:'犬',category:'動物',keywords:['犬','気持ち','しぐさ','特徴'],templates:[['犬の','しぐさを','見ると、','今どんな','気持ちか','想像したく','なります。'],['表情や','動きから、','犬らしさが','よく','伝わります。']]},cat:{label:'猫',category:'動物',keywords:['猫','表情','動き','特徴'],templates:[['猫は','静かな','動きや','表情から、','独特の','魅力が','感じられます。'],['しぐさを','観察すると、','毎日の','面白さが','見えてきます。']]},forest:{label:'森',category:'自然',keywords:['森','木','生き物','空気'],templates:[['森には','多くの','木や','生き物が','いて、','自然の','つながりを','感じられます。'],['静かな','空気の','中で、','季節の','変化も','見つけやすい','場所です。']]},sea:{label:'海',category:'自然',keywords:['海','広さ','波','生き物'],templates:[['海は','広く、','波の','音や','景色から','大きさを','感じられます。'],['多くの','生き物が','いて、','調べるほど','新しい','発見が','あります。']]},mountain:{label:'山',category:'自然',keywords:['山','高さ','景色','自然'],templates:[['山は','高さの','違いに','よって、','見える','景色や','空気が','変わります。'],['登るほど、','自然の','広がりを','強く','感じられます。']]},train:{label:'電車',category:'一般',keywords:['電車','便利さ','路線','移動'],templates:[['電車は','多くの','人の','移動を','支える','大切な','乗り物です。'],['路線の','つながりを','見ると、','街の','仕組みも','分かります。']]},music:{label:'音楽',category:'文化',keywords:['音楽','リズム','表現','気分'],templates:[['音楽は','リズムや','メロディーによって、','気分を','大きく','変える','力が','あります。'],['同じ','曲でも、','聞く','場面に','よって、','受け取り方が','変わります。']]},festival:{label:'お祭り',category:'文化',keywords:['お祭り','にぎわい','地域','伝統'],templates:[['お祭りは','地域の','人が','集まり、','にぎわいを','感じられる','行事です。'],['伝統や','季節の','雰囲気を','知る','きっかけにも','なります。']]},art:{label:'美術',category:'文化',keywords:['美術','表現','色','見方'],templates:[['美術では','色や','形の','使い方によって、','さまざまな','表現が','生まれます。'],['見る人に','よって、','受ける','印象が','変わるのも','面白さです。']]}};
const state={tab:'demo',theoryTab:'basic',isGenerating:false,generationHistory:[],candidateWords:[],wordProbabilities:[],selectedWords:[],thinkingLogs:[],finalSentence:'',aiMood:'待機中',interval:null,currentSentence:[],reasonHistory:[],targetTemplate:[]};
function renderThemeSelect(){const s=document.getElementById('theme-select');s.innerHTML='';Object.entries(topics).forEach(([k,t])=>{const o=document.createElement('option');o.value=k;o.textContent=`${t.label}（${t.category}）`;s.appendChild(o)});s.value='spring'}
function updateTopicView(){const t=topics[document.getElementById('theme-select').value];document.getElementById('view-topic').textContent=t.label;document.getElementById('view-category').textContent=t.category;document.getElementById('view-keywords').textContent=t.keywords.join(' / ');renderProgress()}
function renderProgress(){const total=state.targetTemplate.length||8,current=state.currentSentence.length,area=document.getElementById('progress-area');area.innerHTML='';const wrap=document.createElement('div');wrap.className='progress-wrap';for(let i=0;i<total;i++){const seg=document.createElement('div');seg.className='progress-seg';if(i<current)seg.classList.add('active');else if(i===current&&state.isGenerating)seg.classList.add('current');wrap.appendChild(seg)}const txt=document.createElement('div');txt.style.fontSize='13px';txt.style.color='#64748b';txt.textContent=`${current} / ${total} 単語`;area.appendChild(wrap);area.appendChild(txt)}
function randomProbabilities(len,correctIndex){let base=[];for(let i=0;i<len;i++){base.push((i===correctIndex?55:20)+Math.random()*20)}const total=base.reduce((a,b)=>a+b,0);return base.map(v=>v/total*100)}
function buildCandidateSequence(target,all){const steps=[];for(let i=0;i<target.length;i++){const set=new Set();all.forEach(t=>{if(t[i])set.add(t[i])});set.add(target[i]);let group=[target[i],...Array.from(set).filter(x=>x!==target[i])].slice(0,4);steps.push(group)}return steps}
function buildReasons(topic,chosen,cands,context,next){const reasons=[];if(!context||context==='（文のはじめ）')reasons.push(`文のはじめなので、テーマ「${topic.label}」をすぐに連想しやすい出だしになっています。`);else reasons.push(`直前の文脈「${context}」のあとに置いても、意味の流れが自然に続きやすい単語です。`);if(topic.keywords.some(k=>chosen.includes(k)))reasons.push(`「${chosen}」はテーマの中心語と関係が強く、話題から外れにくい単語です。`);else reasons.push(`「${chosen}」はテーマの説明に必要な役割を持ち、文を前に進めやすい単語です。`);if(next)reasons.push(`このあとに「${next}」のような単語が続きやすく、文全体を作りやすくなります。`);if(cands.length>1){const alts=cands.filter(c=>c!==chosen).slice(0,2);reasons.push(`他にも ${alts.join('・')} などが候補でしたが、今回はいちばん流れが自然なものとして選ばれました。`)}return reasons}
function renderCandidates(){const area=document.getElementById('candidates-area');if(!state.candidateWords.length){area.className='soft-body';area.textContent='生成を開始すると、ここに候補と確率が表示されます。';return}area.className='';area.innerHTML='';state.candidateWords.forEach((group,i)=>{const block=document.createElement('div');block.style.marginBottom='18px';const label=document.createElement('div');label.className='soft-title';label.textContent=`Step ${i+1} の候補`;block.appendChild(label);group.forEach((word,j)=>{const item=document.createElement('div');item.className='cand-item';const row=document.createElement('div');row.className='cand-row';const left=document.createElement('span');left.textContent=word;if(state.selectedWords[i]===word){const tag=document.createElement('span');tag.className='tag';tag.textContent='選択';left.appendChild(tag)}const right=document.createElement('span');right.textContent=`${(state.wordProbabilities[i]?.[j]||0).toFixed(1)}%`;row.append(left,right);const bo=document.createElement('div');bo.className='bar-outer';const bi=document.createElement('div');bi.className='bar-inner';bi.style.width=`${state.wordProbabilities[i]?.[j]||0}%`;if(state.selectedWords[i]!==word)bi.style.background='#94a3b8';bo.appendChild(bi);item.append(row,bo);block.appendChild(item)});area.appendChild(block)})}
function renderThinking(){const area=document.getElementById('thinking-area');if(!state.thinkingLogs.length){area.className='soft-body';area.textContent='生成後に思考プロセスが表示されます。';return}area.className='';area.innerHTML='';state.thinkingLogs.forEach(log=>{const card=document.createElement('div');card.className='log';card.innerHTML=`<div class="log-title">Step ${log.step}</div><div class="soft-body"><div>${log.analysis}</div><div style="margin-top:6px;">確率: ${log.probability.map(p=>p.toFixed(1)).join(' / ')} %</div><div style="margin-top:6px;color:#334155;font-weight:700;">${log.decision}</div></div>`;area.appendChild(card)})}
function renderHistory(){const area=document.getElementById('history-area');if(!state.generationHistory.length){area.className='soft-body';area.textContent='生成履歴はまだありません。';return}area.className='';area.innerHTML='';state.generationHistory.forEach(item=>{const div=document.createElement('div');div.className='history-item';div.textContent=item;area.appendChild(div)})}
function renderCurrentSentence(){const area=document.getElementById('current-sentence');if(!state.currentSentence.length)area.innerHTML='まだ生成されていません<span class="pending"> 次の単語を待っています…</span>';else if(state.isGenerating)area.innerHTML=`${state.currentSentence.join('')}<span class="pending"> 次の単語を待っています…</span>`;else area.textContent=state.currentSentence.join('')}
function renderContextHint(){const area=document.getElementById('current-context');if(!state.currentSentence.length){area.textContent='まだ文が始まっていないので、テーマに最も合いそうな最初の単語候補を比べます。';return}const context=state.currentSentence.join('');area.textContent=`いまの文脈「${context}」のあとに続くと自然そうな単語を比べています。`}
function renderWhyWordHistory(){const area=document.getElementById('why-word-history');if(!state.reasonHistory.length){area.textContent='生成を始めると、ここに選ばれた理由が順番に表示されます。';return}area.innerHTML='';state.reasonHistory.forEach((r,idx)=>{const entry=document.createElement('div');entry.className='reason-entry';entry.innerHTML=`<div class="reason-entry-title">Step ${idx+1}：${r.word}</div><div><strong>その時点の文脈:</strong> ${r.context}</div><div style="margin-top:6px;"><strong>選ばれた理由</strong></div><ul class="reason-list">${r.reasons.map(x=>`<li>${x}</li>`).join('')}</ul><div style="margin-top:8px;"><strong>次につながる見込み</strong></div><div>${r.nextHint}</div>`;area.appendChild(entry)});area.scrollTop=area.scrollHeight}
function renderResult(){document.getElementById('final-sentence').textContent=state.finalSentence||'ここに完成した文が表示されます';document.getElementById('selected-words').textContent=state.selectedWords.length?`今回選ばれた単語: ${state.selectedWords.join(' / ')}`:'まだありません';document.getElementById('ai-mood').textContent=state.aiMood;renderCurrentSentence();renderContextHint();renderWhyWordHistory();renderProgress()}
function renderTabs(){document.getElementById('tab-demo').className='btn'+(state.tab==='demo'?' primary':'');document.getElementById('tab-theory').className='btn'+(state.tab==='theory'?' primary':'');document.getElementById('demo-panel').style.display=state.tab==='demo'?'':'none';document.getElementById('theory-panel').style.display=state.tab==='theory'?'':'none';document.getElementById('subtab-basic').className='btn'+(state.theoryTab==='basic'?' primary':'');document.getElementById('subtab-advanced').className='btn'+(state.theoryTab==='advanced'?' primary':'');document.getElementById('basic-panel').style.display=state.theoryTab==='basic'?'':'none';document.getElementById('advanced-panel').style.display=state.theoryTab==='advanced'?'':'none'}
function generate(){const key=document.getElementById('theme-select').value,topic=topics[key],target=topic.templates[Math.floor(Math.random()*topic.templates.length)],steps=buildCandidateSequence(target,topic.templates),probs=steps.map(g=>randomProbabilities(g.length,0)),logs=[];state.targetTemplate=target.slice();state.isGenerating=true;state.candidateWords=[];state.wordProbabilities=[];state.selectedWords=[];state.thinkingLogs=[];state.finalSentence='';state.aiMood='分析中...';state.currentSentence=[];state.reasonHistory=[];renderResult();renderCandidates();renderThinking();let index=0;if(state.interval)clearInterval(state.interval);state.interval=setInterval(()=>{if(index>=steps.length){clearInterval(state.interval);state.finalSentence=target.join('');state.thinkingLogs=logs;state.generationHistory=[state.finalSentence,...state.generationHistory].slice(0,8);state.aiMood='完了';state.isGenerating=false;renderResult();renderThinking();renderHistory();return}const currentContext=state.currentSentence.join('')||'（文のはじめ）';const chosen=steps[index][0];const nextToken=target[index+1]||'文の終わり';const reasons=buildReasons(topic,chosen,steps[index],currentContext,nextToken==='文の終わり'?null:nextToken);state.aiMood=`「${chosen}」を処理中`;state.candidateWords.push(steps[index]);state.wordProbabilities.push(probs[index]);state.selectedWords.push(chosen);state.currentSentence.push(chosen);state.reasonHistory.push({word:chosen,context:currentContext,reasons:reasons,nextHint:nextToken==='文の終わり'?'この単語で文が自然にまとまり、ここで完結できます。':`このあとには「${nextToken}」のような単語が続くと、さらに自然な文になります。`});logs.push({step:index+1,analysis:`いまの文脈 ${currentContext} のあとに続く単語候補を比べています。`,probability:probs[index],decision:`今回は「${chosen}」を選びました。`});renderResult();renderCandidates();index+=1},700)}
document.getElementById('tab-demo').addEventListener('click',()=>{state.tab='demo';renderTabs()});document.getElementById('tab-theory').addEventListener('click',()=>{state.tab='theory';renderTabs()});document.getElementById('subtab-basic').addEventListener('click',()=>{state.theoryTab='basic';renderTabs()});document.getElementById('subtab-advanced').addEventListener('click',()=>{state.theoryTab='advanced';renderTabs()});document.getElementById('generate-btn').addEventListener('click',generate);document.getElementById('theme-select').addEventListener('change',updateTopicView);renderThemeSelect();updateTopicView();renderTabs();renderResult();renderCandidates();renderThinking();renderHistory();
</script>
</body>
</html>
