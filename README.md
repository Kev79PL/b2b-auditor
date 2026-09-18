# Beauty Auditor — audyt.spacepr.pl

Narzędzie do analizy i porównania stron beauty z konkurencją. Backend: Flask + Gemini 2.5 Flash. Hosting: Render.

## Zmienne środowiskowe (Render → Environment)

| Zmienna | Opis |
|---|---|
| `GEMINI_API_KEY` | Klucz API Google Gemini (console.cloud.google.com) |
| `N8N_WEBHOOK_URL` | Webhook n8n — nowe zamówienia i leady z darmowego audytu |
| `N8N_WEBHOOK_PLATNOSC_URL` | Webhook n8n — zdarzenia po udanej płatności Stripe |
| `STRIPE_WEBHOOK_SECRET` | Sekret do weryfikacji webhooków Stripe (`whsec_...`) |
| `STRIPE_SECRET_KEY` | Tajny klucz Stripe (`sk_live_...` lub `sk_test_...`) |

## Stripe webhook

Po skonfigurowaniu produktu w Stripe Dashboard ustaw endpoint webhooka:

```
https://audyt.spacepr.pl/webhook/stripe
```

Zdarzenie do nasłuchiwania: `checkout.session.completed`

## Render — unikanie cold startu (free tier)

Jeśli korzystasz z planu free, serwer zasypia po 15 minutach bezczynności.
Strona `index.html` wysyła automatyczny ping co 5 minut gdy jest otwarta w przeglądarce.

**Alternatywa:** na planie paid włącz "Always On" w ustawieniach serwisu na Render.

## Uruchomienie lokalne

```bash
pip install -r requirements.txt
GEMINI_API_KEY=twoj_klucz python3 server.py
```

Serwer działa na `http://localhost:5001`.

## Generator Strategii Marketingowej — `strategia/index.html`

Samodzielne narzędzie szkoleniowe (SPA w jednym pliku: HTML + Tailwind CSS z CDN + Vanilla JS).
Uczestnik wpisuje dane o firmie, grupie docelowej, celu, budżecie i kanałach, a aplikacja
generuje raport: profil grupy docelowej, ton komunikacji, dobór narzędzi reklamowych,
podział budżetu w lejku, wskazówki do analizy konkurencji i plan 90 dni.

- Działa bez backendu — wystarczy otworzyć `strategia/index.html` w przeglądarce (wymaga internetu do pobrania Tailwind, Lucide i fontów z CDN).
- Rekomendacje są budowane z reguł (baza wiedzy w sekcji `BAZA WIEDZY` w pliku) — logikę dopasowania można edytować bez znajomości frameworków.
- Eksport: „Drukuj / zapisz PDF” (`window.print()` ze stylami `@media print`) oraz „Kopiuj podsumowanie” (tekst do schowka).
- Formularz zapisuje się lokalnie w `localStorage`; przycisk „Wczytaj przykład” wypełnia dane demonstracyjne.
