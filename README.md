<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>냥이의 철강 무게 계산기 - 오프라인 수정판</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans KR", Arial, sans-serif;
      background: #f3f4f6;
      color: #1f2937;
    }
    .page {
      min-height: 100vh;
      padding: 24px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .card {
      width: 100%;
      max-width: 760px;
      background: #fff;
      border-radius: 24px;
      box-shadow: 0 18px 45px rgba(15, 23, 42, 0.14);
      border-top: 8px solid #fb923c;
      padding: 32px;
    }
    .title { text-align: center; margin-bottom: 28px; }
    .title h1 { margin: 0 0 8px; font-size: 30px; }
    .title p { margin: 0; color: #6b7280; }
    .grid2 { display: grid; grid-template-columns: repeat(2, 1fr); gap: 18px; }
    .field label {
      display: block;
      font-size: 13px;
      font-weight: 800;
      margin-bottom: 7px;
      color: #374151;
    }
    select, input {
      width: 100%;
      padding: 12px 13px;
      border: 1px solid #d1d5db;
      border-radius: 10px;
      font-size: 16px;
      outline: none;
      background: #fff;
    }
    select:focus, input:focus { border-color: #fb923c; box-shadow: 0 0 0 3px rgba(251, 146, 60, 0.18); }
    .box {
      margin-top: 24px;
      background: #fff7ed;
      border: 1px solid #fed7aa;
      border-radius: 18px;
      padding: 22px;
    }
    .box h2 {
      margin: 0 0 18px;
      padding-bottom: 10px;
      border-bottom: 1px solid #fed7aa;
      color: #9a3412;
      font-size: 19px;
    }
    .help { font-size: 12px; color: #6b7280; margin: 7px 0 14px; }
    .price-row { display: flex; align-items: center; gap: 14px; }
    .add-btn {
      width: 54px;
      height: 54px;
      border-radius: 50%;
      border: 0;
      background: #fff;
      color: #ef4444;
      font-size: 42px;
      line-height: 1;
      cursor: pointer;
      transition: 0.15s;
    }
    .add-btn:hover { background: #fee2e2; color: #dc2626; }
    .result {
      margin-top: 24px;
      background: #1e293b;
      color: #fff;
      border-radius: 18px;
      padding: 30px;
      text-align: center;
    }
    .result .label { color: #94a3b8; font-size: 13px; font-weight: 800; letter-spacing: 0.05em; }
    .weight { margin-top: 8px; font-size: 60px; font-weight: 950; color: #fb923c; }
    .unit { font-size: 26px; color: #d1d5db; margin-left: 4px; }
    .money-box { margin-top: 22px; padding-top: 20px; border-top: 1px solid #334155; }
    .money { margin-top: 7px; font-size: 42px; font-weight: 950; color: #60a5fa; }
    .note { margin-top: 20px; color: #64748b; font-size: 12px; }
    .list-box {
      margin-top: 24px;
      background: #fff;
      border: 1px solid #e5e7eb;
      border-radius: 18px;
      padding: 20px;
      box-shadow: 0 4px 12px rgba(15, 23, 42, 0.04);
    }
    .list-box h2 { margin: 0 0 12px; font-size: 18px; border-bottom: 1px solid #e5e7eb; padding-bottom: 10px; }
    .item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      padding: 10px;
      background: #f9fafb;
      border: 1px solid #f3f4f6;
      border-radius: 10px;
      margin-bottom: 8px;
    }
    .item-main { font-size: 14px; }
    .item-main .num { color: #ea580c; font-weight: 900; margin-right: 6px; }
    .item-main .strong { font-weight: 900; margin-right: 6px; }
    .dim { display: block; color: #6b7280; font-size: 12px; margin-top: 3px; }
    .item-side { display: flex; align-items: center; gap: 12px; white-space: nowrap; }
    .item-weight { font-weight: 900; text-align: right; }
    .item-price { font-weight: 900; color: #2563eb; font-size: 12px; text-align: right; }
    .remove { border: 0; background: transparent; color: #9ca3af; cursor: pointer; font-size: 18px; }
    .remove:hover { color: #ef4444; }
    .sum-box { margin-top: 14px; background: #ffedd5; border: 1px solid #fed7aa; border-radius: 14px; padding: 14px; }
    .sum-line { display: flex; justify-content: space-between; align-items: center; font-weight: 900; color: #7c2d12; }
    .sum-line + .sum-line { border-top: 1px solid #fed7aa; margin-top: 10px; padding-top: 10px; }
    .sum-value { color: #ea580c; font-size: 22px; }
    .sum-money { color: #1d4ed8; font-size: 22px; }
    .hidden { display: none; }
    @media (max-width: 640px) {
      .page { padding: 12px; align-items: flex-start; }
      .card { padding: 22px; border-radius: 18px; }
      .title h1 { font-size: 23px; }
      .grid2 { grid-template-columns: 1fr; }
      .weight { font-size: 46px; }
      .money { font-size: 32px; }
      .item { align-items: flex-start; flex-direction: column; }
      .item-side { width: 100%; justify-content: space-between; }
    }
  </style>
</head>
<body>
  <div class="page">
    <main class="card">
      <div class="title">
        <h1>🐾 냥이의 철강 무게 계산기 🐾</h1>
        <p>치수를 입력하면 무게와 금액을 바로 계산한다냥!</p>
      </div>

      <section class="grid2">
        <div class="field">
          <label for="material">재질 고르기</label>
          <select id="material">
            <option value="ss400">SS400 (일반철강 - 비중 7.85)</option>
            <option value="sus304">SUS304 (스텐 - 비중 7.93)</option>
          </select>
        </div>
        <div class="field">
          <label for="shape">모양 고르기</label>
          <select id="shape">
            <option value="plate">판재 (Plate)</option>
            <option value="angle">앵글 (Angle)</option>
            <option value="hbeam">H빔 (H-Beam)</option>
            <option value="channel">잔넬 (Channel)</option>
          </select>
        </div>
      </section>

      <section class="box">
        <h2>📏 치수 입력 (단위: mm)</h2>

        <div id="plateInputs" class="grid2">
          <div class="field">
            <label for="W">W (폭/가로)</label>
            <input type="number" id="W" value="1000" step="0.1" />
          </div>
          <div class="field">
            <label for="t1">t (두께)</label>
            <input type="number" id="t1" value="1.6" step="0.1" />
          </div>
        </div>

        <div id="angleInputs" class="grid2 hidden">
          <div class="field">
            <label for="A">A (가로)</label>
            <input type="number" id="A" value="50" step="0.1" />
          </div>
          <div class="field">
            <label for="B">B (세로)</label>
            <input type="number" id="B" value="50" step="0.1" />
          </div>
          <div class="field">
            <label for="tAngle">t (두께)</label>
            <input type="number" id="tAngle" value="1.6" step="0.1" />
          </div>
        </div>

        <div id="beamInputs" class="grid2 hidden">
          <div class="field">
            <label for="H">H (높이)</label>
            <input type="number" id="H" value="100" step="0.1" />
          </div>
          <div class="field">
            <label for="beamW">B (폭)</label>
            <input type="number" id="beamW" value="100" step="0.1" />
          </div>
          <div class="field">
            <label for="webT">t1 (웨브 두께)</label>
            <input type="number" id="webT" value="1.6" step="0.1" />
          </div>
          <div class="field">
            <label for="flangeT">t2 (플랜지 두께)</label>
            <input type="number" id="flangeT" value="7" step="0.1" />
          </div>
        </div>

        <div class="field" style="margin-top:16px;">
          <label for="L">L (전체 길이, mm)</label>
          <input type="number" id="L" value="2000" step="0.1" />
          <p class="help">※ 1m는 1000mm다냥.</p>
        </div>

        <div class="field">
          <label for="pricePerKg">단가 (kg당 금액, 원)</label>
          <div class="price-row">
            <input type="number" id="pricePerKg" value="0" step="1" placeholder="예: 1200" />
            <button id="addBtn" class="add-btn" title="목록에 추가">+</button>
          </div>
          <p class="help" style="color:#2563eb;">※ 단가를 넣고 +를 누르면 총 금액도 같이 계산한다냥.</p>
        </div>
      </section>

      <section id="listBox" class="list-box hidden">
        <h2>📋 계산 목록</h2>
        <div id="itemList"></div>
        <div class="sum-box">
          <div class="sum-line"><span>총 무게 합계</span><span id="totalWeight" class="sum-value">0.00 kg</span></div>
          <div class="sum-line"><span>총 금액 합계</span><span id="totalPrice" class="sum-money">0 원</span></div>
        </div>
      </section>

      <section class="result">
        <div class="label">현재 입력된 예상 무게</div>
        <div><span id="weight" class="weight">0.00</span><span class="unit">kg</span></div>

        <div id="moneyBox" class="money-box hidden">
          <div class="label">현재 입력된 예상 금액</div>
          <div><span id="money" class="money">0</span><span class="unit">원</span></div>
        </div>

        <p class="note">* R값, 모서리 라운드, 실제 KS 규격 공차는 제외한 단순 단면적 계산이다냥.</p>
      </section>
    </main>
  </div>

  <script>
    const densities = { ss400: 7.85, sus304: 7.93 };
    const materialName = { ss400: 'SS400', sus304: 'SUS304' };
    const shapeName = { plate: '판재', angle: '앵글', hbeam: 'H빔', channel: '잔넬' };
    const items = [];

    const $ = (id) => document.getElementById(id);
    const num = (id) => parseFloat($(id).value) || 0;
    const won = (value) => Math.round(value).toLocaleString('ko-KR');

    function getCurrentData() {
      const material = $('material').value;
      const shape = $('shape').value;
      const L = num('L');
      let area = 0;
      let dimText = '';

      if (shape === 'plate') {
        const W = num('W');
        const t = num('t1');
        area = W * t;
        dimText = `W:${W}, t:${t}, L:${L}`;
      }

      if (shape === 'angle') {
        const A = num('A');
        const B = num('B');
        const t = num('tAngle');
        area = (A + B - t) * t;
        dimText = `A:${A}, B:${B}, t:${t}, L:${L}`;
      }

      if (shape === 'hbeam' || shape === 'channel') {
        const H = num('H');
        const W = num('beamW');
        const t1 = num('webT');
        const t2 = num('flangeT');
        area = (2 * W * t2) + ((H - 2 * t2) * t1);
        dimText = `H:${H}, B:${W}, t1:${t1}, t2:${t2}, L:${L}`;
      }

      const volume = area * L;
      const weight = Math.max((volume * densities[material]) / 1000000, 0);
      const pricePerKg = num('pricePerKg');
      const totalPrice = weight * pricePerKg;

      return { material, shape, L, area, weight, pricePerKg, totalPrice, dimText };
    }

    function updateInputVisibility() {
      const shape = $('shape').value;
      $('plateInputs').classList.toggle('hidden', shape !== 'plate');
      $('angleInputs').classList.toggle('hidden', shape !== 'angle');
      $('beamInputs').classList.toggle('hidden', !(shape === 'hbeam' || shape === 'channel'));
    }

    function calculate() {
      updateInputVisibility();
      const data = getCurrentData();
      $('weight').textContent = data.weight.toFixed(2);
      $('money').textContent = won(data.totalPrice);
      $('moneyBox').classList.toggle('hidden', data.pricePerKg <= 0);
    }

    function renderItems() {
      const listBox = $('listBox');
      const itemList = $('itemList');
      listBox.classList.toggle('hidden', items.length === 0);
      itemList.innerHTML = '';

      items.forEach((item, index) => {
        const row = document.createElement('div');
        row.className = 'item';
        row.innerHTML = `
          <div class="item-main">
            <span class="num">#${index + 1}</span>
            <span class="strong">[${materialName[item.material]}]</span>
            <span class="strong">${shapeName[item.shape]}</span>
            <span class="dim">(${item.dimText})</span>
          </div>
          <div class="item-side">
            <div>
              <div class="item-weight">${item.weight.toFixed(2)} kg</div>
              ${item.totalPrice > 0 ? `<div class="item-price">${won(item.totalPrice)} 원</div>` : ''}
            </div>
            <button class="remove" data-id="${item.id}" title="삭제">✕</button>
          </div>
        `;
        itemList.appendChild(row);
      });

      const totalWeight = items.reduce((sum, item) => sum + item.weight, 0);
      const totalPrice = items.reduce((sum, item) => sum + item.totalPrice, 0);
      $('totalWeight').textContent = `${totalWeight.toFixed(2)} kg`;
      $('totalPrice').textContent = `${won(totalPrice)} 원`;
    }

    function addItem() {
      const data = getCurrentData();
      if (data.weight <= 0) {
        alert('무게가 0kg이라 목록에 추가할 수 없다냥. 치수를 확인해줘냥!');
        return;
      }
      items.push({ id: Date.now(), ...data });
      renderItems();
      calculate();
    }

    document.addEventListener('input', calculate);
    document.addEventListener('change', calculate);
    $('addBtn').addEventListener('click', addItem);
    $('itemList').addEventListener('click', (e) => {
      if (!e.target.classList.contains('remove')) return;
      const id = Number(e.target.dataset.id);
      const index = items.findIndex((item) => item.id === id);
      if (index >= 0) items.splice(index, 1);
      renderItems();
    });

    calculate();
  </script>
</body>
</html>
