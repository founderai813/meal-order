// ════════════════════════════════════════════════════
//  團體訂餐系統 — Google Apps Script 後端
//  使用 PropertiesService 儲存 + Gemini AI 辨識菜單
// ════════════════════════════════════════════════════
 
const GEMINI_API_KEY = 'AIzaSyDiM2CClCx2rivVlv1KRTH6qgJiXBn_jfU';
const GEMINI_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=' + GEMINI_API_KEY;
 
function doGet(e)  { return handle(e); }
function doPost(e) { return handle(e); }
 
function handle(e) {
  try {
    const body   = e.postData ? JSON.parse(e.postData.contents) : {};
    const action = e.parameter.action || body.action || '';
 
    let result;
    if      (action === 'ping')             result = { ok: true };
    else if (action === 'create')           result = createOrder(body);
    else if (action === 'read')             result = readOrder(e.parameter.key || body.key);
    else if (action === 'update')           result = updateOrder(body);
    else if (action === 'parseMenu')        result = parseMenuWithAI(body);
    else if (action === 'parseMenuImage')   result = parseMenuWithImage(body);
    else                                    result = { ok: false, error: 'Unknown action: ' + action };
 
    return out(result);
  } catch(err) {
    return out({ ok: false, error: err.message });
  }
}
 
function out(obj) {
  return ContentService
    .createTextOutput(JSON.stringify(obj))
    .setMimeType(ContentService.MimeType.JSON);
}
 
function store() {
  return PropertiesService.getScriptProperties();
}
 
// ── 建立新訂單 ──
function createOrder(body) {
  const key  = 'ord_' + Date.now() + '_' + Math.random().toString(36).slice(2, 6);
  const data = JSON.stringify({
    orderInfo: body.orderInfo || {},
    menuItems: body.menuItems || [],
    orders:    [],
    isClosed:  false
  });
  store().setProperty(key, data);
  return { ok: true, key: key };
}
 
// ── 讀取訂單 ──
function readOrder(key) {
  if (!key) return { ok: false, error: '缺少 key' };
  const raw = store().getProperty(key);
  if (!raw)  return { ok: false, error: '找不到此訂單' };
  return { ok: true, data: JSON.parse(raw) };
}
 
// ── 更新訂單 ──
function updateOrder(body) {
  const key = body.key;
  if (!key) return { ok: false, error: '缺少 key' };
  if (!store().getProperty(key)) return { ok: false, error: '找不到此訂單' };
  store().setProperty(key, JSON.stringify(body.data));
  return { ok: true };
}
 
// ── Gemini AI 辨識菜單（文字）──
function parseMenuWithAI(body) {
  const text = body.text || '';
  if (!text) return { ok: false, error: '沒有收到菜單內容' };
 
  const prompt = `以下是從菜單文件中擷取的文字內容，請找出所有餐點名稱和對應的價格。
只回傳 JSON 格式，不要任何說明：{"items":[{"name":"餐點名稱","price":數字},...]}
價格只取數字（不含$符號）。如果一個餐點有多種價格，取第一個價格。
忽略類別標題、備註、熱量等非餐點資訊。
 
菜單內容：
${text}`;
 
  try {
    const response = UrlFetchApp.fetch(GEMINI_URL, {
      method: 'POST',
      contentType: 'application/json',
      payload: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }],
        generationConfig: { temperature: 0.1, maxOutputTokens: 2048 }
      })
    });
    const result = JSON.parse(response.getContentText());
    const raw = result.candidates[0].content.parts[0].text;
    const clean = raw.replace(/```json|```/g, '').trim();
    const parsed = JSON.parse(clean);
    return { ok: true, items: parsed.items || [] };
  } catch(e) {
    return { ok: false, error: 'AI 辨識失敗：' + e.message };
  }
}
 
// ── Gemini AI 辨識菜單（圖片/PDF）──
function parseMenuWithImage(body) {
  const base64 = body.base64 || '';
  const mimeType = body.mimeType || 'image/jpeg';
  if (!base64) return { ok: false, error: '沒有收到圖片內容' };
 
  const prompt = `這是一份菜單的圖片或PDF，請找出所有餐點名稱和對應的價格。
只回傳 JSON 格式，不要任何說明：{"items":[{"name":"餐點名稱","price":數字},...]}
價格只取數字（不含$符號）。如果一個餐點有多種價格，取第一個價格。
忽略類別標題、備註、熱量等非餐點資訊。`;
 
  try {
    const response = UrlFetchApp.fetch(GEMINI_URL, {
      method: 'POST',
      contentType: 'application/json',
      payload: JSON.stringify({
        contents: [{
          parts: [
            { inline_data: { mime_type: mimeType, data: base64 } },
            { text: prompt }
          ]
        }],
        generationConfig: { temperature: 0.1, maxOutputTokens: 2048 }
      })
    });
    const result = JSON.parse(response.getContentText());
    const raw = result.candidates[0].content.parts[0].text;
    const clean = raw.replace(/```json|```/g, '').trim();
    const parsed = JSON.parse(clean);
    return { ok: true, items: parsed.items || [] };
  } catch(e) {
    return { ok: false, error: 'AI 辨識失敗：' + e.message };
  }
}
