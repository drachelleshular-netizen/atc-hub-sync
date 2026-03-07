export default async function handler(req, res) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
  
  if (req.method === 'OPTIONS') return res.status(200).end();

  const BIN_ID = '69ab4ed2ae596e708f674237';
  const BIN_KEY = '$2a$10$1qA1M/f9hUz3qAnfqagqRuG1upzRuOhB41beRa327EKYZSeCr2rse';
  const BIN_URL = `https://api.jsonbin.io/v3/b/${BIN_ID}`;

  if (req.method === 'GET') {
    const r = await fetch(`${BIN_URL}/latest`, {
      headers: { 'X-Master-Key': BIN_KEY }
    });
    const data = await r.json();
    return res.status(200).json(data.record || {});
  }

  if (req.method === 'POST') {
    const r = await fetch(BIN_URL, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json', 'X-Master-Key': BIN_KEY },
      body: JSON.stringify(req.body)
    });
    const data = await r.json();
    return res.status(200).json({ ok: !!data.record });
  }

  return res.status(400).json({ ok: false });
}
```

Click **Commit changes**. Vercel will automatically redeploy in about 30 seconds.

Then test it by visiting this URL in your browser:
```
https://atc-hub-sync-bj6a.vercel.app/api/sync
