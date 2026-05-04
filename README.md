# Qashqadaryo viloyati xaritasi

Qashqadaryo viloyatining interaktiv kartasi: 14 tuman + 2 shahar (Qarshi, Shahrisabz), 776 mahalla (MFY), ko'chalar va yo'l kodlari ro'yxati.

## Imkoniyatlari

- Region chegarasi, tumanlar va mahallalar (MFY) Leaflet kartada
- Tumanni tanlash (yuqoridagi `<select>` orqali)
- Mahallani bosish — info panel ochiladi
- **Ko'chalar va yo'l kodlari** — har bir mahalla uchun (`streets.json`)
- **Qidiruv** — mahalla nomi, ko'cha nomi va yo'l kodi bo'yicha
- **Yangi ko'cha qo'shish** — info panel ichidan (lokal saqlanadi, doimiy qilish uchun `streets.json` ga ko'chiring)
- Onlayn xarita (OpenStreetMap, CARTO Light, Esri sun'iy yo'ldosh)

## Ishga tushirish

Loyihada build kerak emas. Lekin `streets.json` faylini `fetch` qilish uchun lokal HTTP server kerak (brauzerda `file://` orqali ochish ishlamaydi):

```bash
python3 -m http.server 8080
# yoki
npx serve .
```

So'ngra brauzerda <http://localhost:8080> ni oching.

## Ko'chalar ma'lumoti — `streets.json`

Asosiy ko'chalar va yo'l kodlari `streets.json` faylida saqlanadi. Tuzilishi:

```json
{
  "streets_by_mahalla": {
    "1710242066": [
      { "name": "Mustaqillik ko'chasi", "road_code": "QSH-001" },
      { "name": "Alisher Navoiy ko'chasi", "road_code": "QSH-002" }
    ],
    "1710242081": [
      { "name": "Bog'iston ko'chasi", "road_code": "CHR-005" }
    ]
  }
}
```

- Kalit (key) — `mahalla_id` (string sifatida).
- Qiymat — ko'chalar massivi, har biri `{ name, road_code }`.
- `mahalla_id` ni ilova ichida har bir mahallani bosib ko'rish mumkin (info panelda chiqadi) yoki `mahallas.geojson` faylidan o'qib olish mumkin.

### Yangi ko'cha qo'shishning ikki yo'li

**1. Tezkor (lokal — faqat sizning brauzeringizda saqlanadi):**

Mahallani xaritada bosing, info panelda quyida joylashgan formaga ko'cha nomi va yo'l kodini kiriting va `+ Qo'shish` tugmasini bosing. Bu ma'lumotlar `localStorage` ga saqlanadi va boshqa foydalanuvchilarga ko'rinmaydi.

**2. Doimiy (hamma uchun — `streets.json` ga yozish):**

`streets.json` faylini matn muharririda oching va kerakli `mahalla_id` ostiga yangi ob'ekt qo'shing:

```json
"1710242066": [
  { "name": "Mavjud ko'cha", "road_code": "QSH-001" },
  { "name": "Yangi ko'cha", "road_code": "QSH-099" }
]
```

JSON sintaksisiga e'tibor bering — vergullar va qavslar to'g'ri yopilgan bo'lishi kerak. O'zgarishlardan keyin sahifani yangilang.

## Fayl tuzilishi

```
Q-xarita/
├── index.html              # Asosiy sahifa (Leaflet karta, UI, JS)
├── data.js                 # boundary + districts + mahallas (window.APP_DATA)
├── streets.json            # Ko'chalar va yo'l kodlari (mahalla_id bo'yicha)
├── boundary.geojson        # Region chegarasi (manba)
├── districts.geojson       # 16 ta tuman/shahar (manba)
├── mahallas.geojson        # 776 ta MFY (manba)
└── README.md
```

## Texnologiyalar

- HTML5 + vanilla JavaScript (build kerak emas)
- [Leaflet 1.9.4](https://leafletjs.com/) — kartalar
- OpenStreetMap, CARTO Light, Esri tile provayderlari
