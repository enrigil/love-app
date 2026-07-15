# 📋 Supabase Setup – Enrique & Sarah Love App

## 1️⃣ Supabase Projekt erstellen

1. Gehe auf **https://supabase.com** und erstelle einen kostenlosen Account
2. Erstelle ein neues Projekt (Name z.B. "love-app")
3. Warte bis das Projekt bereit ist (~1-2 Minuten)

---

## 2️⃣ Tabelle "messages" erstellen

Gehe in deinem Supabase-Projekt zu **Table Editor → New Table**:

| Spaltenname  | Typ          | Default           | Einstellungen          |
|-------------|--------------|-------------------|------------------------|
| `id`        | `int8`       | –                 | Primary Key, Identity  |
| `created_at`| `timestamptz`| `now()`           |                        |
| `sender`    | `text`       | –                 | NOT NULL               |
| `text`      | `text`       | –                 | NOT NULL               |

### Oder via SQL Editor:

```sql
-- Tabelle erstellen
CREATE TABLE public.messages (
  id         BIGSERIAL PRIMARY KEY,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  sender     TEXT        NOT NULL CHECK (sender IN ('enrique', 'sarah')),
  text       TEXT        NOT NULL
);

-- Row Level Security (RLS) aktivieren
ALTER TABLE public.messages ENABLE ROW LEVEL SECURITY;

-- Policy: Jeder kann lesen
CREATE POLICY "Alle können lesen"
  ON public.messages FOR SELECT
  USING (true);

-- Policy: Jeder kann schreiben (für die App ohne Auth)
CREATE POLICY "Jeder kann schreiben"
  ON public.messages FOR INSERT
  WITH CHECK (true);
```

---

## 3️⃣ API Keys holen

Gehe zu **Settings → API**:

- **Project URL**: Sieht aus wie `https://xxxxxxxxxxx.supabase.co`
- **anon public Key**: Der lange `eyJ...` String unter "Project API keys"

---

## 4️⃣ In der App eintragen

1. Öffne die Love App
2. Gehe zu **Nachrichten** (💌 Tab)
3. Trage dort **Supabase URL** und **Anon Key** ein
4. Tippe auf **Speichern & Verbinden**

Die Keys werden sicher im LocalStorage deines Handys gespeichert und müssen nur einmal auf jedem Gerät eingetragen werden.

---

## 5️⃣ Realtime aktivieren

Damit Nachrichten sofort ankommen:

1. Gehe zu **Database → Replication**
2. Aktiviere die `messages` Tabelle für Realtime

---

## ✅ Fertig!

Nach dem Setup können Enrique und Sarah sich in Echtzeit Nachrichten schicken. Die App funktioniert auch **ohne Supabase** mit lokalem Speicher (nur auf einem Gerät).

---

## 🔒 Sicherheitshinweis

Der `anon` Key ist für öffentliche Apps gedacht. Da diese App privat ist und keine sensiblen Daten enthält, ist das in Ordnung. Teile die Keys nicht öffentlich.
