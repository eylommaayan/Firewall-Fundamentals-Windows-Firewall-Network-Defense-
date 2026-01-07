

# 🔥 Firewall Fundamentals – Windows Firewall & Network Defense

> 📘Windows

---

## 🎯 מטרות המסמך

המטרה של מסמך זה היא:

* להבין **מהי חומת אש באמת** (ולא רק הגדרה תאורטית)
* להבין **למה קיימים סוגים שונים של Firewalls**
* להבין **איך תוקפים מנסים לעקוף חומות אש**
* לדעת **איך להגדיר, לבדוק ולנתח Firewall בפועל**
* לחבר בין **הגנה תאורטית** לבין **מצב אמיתי בשטח (PT / SOC)**

---

## 🔐 מהי חומת אש (Firewall)?

חומת אש היא מנגנון אבטחה שתפקידו **לבקר, לסנן ולנטר תעבורת רשת** בין מחשבים או בין רשתות.

היא:

* בודקת תעבורת **Inbound** (נכנסת)
* בודקת תעבורת **Outbound** (יוצאת)
* מחליטה האם **לאפשר או לחסום** תעבורה לפי חוקים מוגדרים

### ❓ למה זה חשוב?

בלי חומת אש:

* כל פורט פתוח = יעד לתקיפה
* כל שירות רץ = נקודת חולשה פוטנציאלית
* אין בקרה על **Data Exfiltration** (דליפת מידע החוצה)

---

## 🔁 Inbound & Outbound – למה מגנים על שניהם?

### ⬅ Inbound (תעבורה נכנסת)

✔ מונע חדירה חיצונית

**דוגמה:**
האקר מנסה להתחבר ל־RDP (פורט 3389) ממדינה זרה → נחסם

---

### ➡ Outbound (תעבורה יוצאת)

✔ מונע נזק מבפנים

**דוגמה:**
Malware הצליח להיכנס למחשב → מנסה לשלוח מידע לשרת C2
Firewall חוסם את היציאה → אין דליפת מידע

> 🔴 הרבה ארגונים מתמקדים רק ב־Inbound – זו טעות קריטית

---

## 🧱 סוגי חומות אש

---

## 1️⃣ Packet Filtering Firewall

בודקת פאקטות בודדות לפי:

* IP מקור
* IP יעד
* פורט
* פרוטוקול

### ✔ יתרונות

* מהירה מאוד
* קלה ליישום

### ✘ חסרונות

* אין הבנה של רצף תעבורה
* לא מזהה מתקפות מתמשכות

**דוגמה אמיתית:**
Slow Port Scan – כל פאקטה נראית חוקית → החומה לא מזהה סריקה

---

## 2️⃣ Stateless Firewall

### מה זה אומר?

* כל פאקטה נבדקת בנפרד
* אין זיכרון
* אין הבנה של “שיחה” בין מחשבים

### ✔ יתרונות

* מהירה
* חסכונית במשאבים

### ✘ חסרונות

* לא מזהה מתקפות מבוססות חיבור

**דוגמה:**
שליחת פאקטות TCP מפוזרות → כל אחת נראית תקינה → נבנה חיבור זדוני

---

## 3️⃣ Stateful Firewall

### איך זה עובד?

* שומרת **State Table**
* יודעת מי התחיל חיבור
* יודעת אם פאקטה שייכת לחיבור קיים

### ✔ יתרונות

* יעילה נגד מתקפות מבוססות חיבורים

**דוגמה:**
פאקטת TCP ACK בלי SYN → מזוהה כלא חוקית → נחסמת

### ✘ חסרונות

* צורכת יותר משאבים (RAM / CPU)
* פחות סקיילבילית ברשתות ענק

---

<img width="403" height="606" alt="image" src="https://github.com/user-attachments/assets/8b9daff0-5f17-42a0-aaea-06c2fc0cad4f" />

## 4️⃣ Web Application Firewall (WAF)

מגינה על **שכבת האפליקציה (Layer 7)**

✔ מגינה מפני:

* SQL Injection
* XSS
* CSRF

```sql
' OR 1=1 --
```

→ WAF מזהה דפוס זדוני → חוסם גם אם הפורט פתוח

---

## 5️⃣ Proxy Firewall

### איך זה עובד?

Client → Proxy → Server

✔ יתרונות:

* הסתרת הרשת הפנימית
* לוגים מלאים
* בידוד

✘ חסרון:

* אם ה־Proxy נופל – אין תקשורת

---

## 6️⃣ NGFW – Next Generation Firewall

כולל:

* Deep Packet Inspection
* זיהוי אפליקציות
* זיהוי התנהגות חריגה
* Threat Intelligence

**דוגמה:**
תקשורת על פורט 443 שלא נראית כמו HTTPS → נחסמת

---

## ⚖ Stateful vs Stateless – סיכום

| נושא           | Stateless | Stateful |
| -------------- | --------- | -------- |
| זיכרון חיבורים | ❌         | ✔        |
| זיהוי מתקפות   | נמוך      | גבוה     |
| ביצועים        | גבוהים    | בינוניים |
| רמת אבטחה      | בסיסית    | מתקדמת   |

---

## 🪟 Windows Defender Firewall

Firewall מובנה ב־Windows
מבוסס **Stateful Firewall**

---

## 🧱 Windows Defender Firewall with Advanced Security
<img width="666" height="477" alt="image" src="https://github.com/user-attachments/assets/0c08bc52-aece-461f-b816-de8f85fbf54e" />
<img width="621" height="446" alt="image" src="https://github.com/user-attachments/assets/65aae6d3-74cb-436c-a78f-45fb43fa4509" />



### תפריט שמאלי

#### 🔹 Inbound Rules

* חוקים לתעבורה נכנסת
* ברירת מחדל: אם אין חוק → נחסם

#### 🔹 Outbound Rules

* חוקים לתעבורה יוצאת
* ברירת מחדל: מאושר
  ⚠ נקודה קריטית ל־Data Exfiltration

#### 🔹 Connection Security Rules

* חוקים מבוססי IPsec
* נפוץ בדומיינים ארגוניים

#### 🔹 Monitoring

* צפייה בחוקים פעילים
* תעבורה חסומה / מאושרת

---

## 🟦 פרופילי רשת

* **Domain** – מנוהל ע"י Domain Controller
* **Private** – רשת ביתית / משרד קטן
* **Public** – רשת ציבורית (הכי נוקשה)

---

## 🧱 כתיבת חוק Firewall – עקרונות

1️⃣ שם ברור
2️⃣ שיוך לקבוצה
3️⃣ בחירת פרופיל (Public / Private / Domain)

---

## 🧪 תרגול – חסימת Web Outbound

```cmd
netsh advfirewall firewall add rule name=Web_Block dir=out action=block protocol=TCP remoteport=80,443
```

### מה החוק עושה?

* חוסם HTTP / HTTPS
* מונע גלישה
* מונע Data Exfiltration

📌 זה נקרא: **Outbound / Egress Filtering**

---

## 🔍 בדיקה ומחיקה

```cmd
netsh advfirewall firewall show rule name=Web_Block
netsh advfirewall firewall delete rule name=Web_Block

<img width="916" height="513" alt="image" src="https://github.com/user-attachments/assets/b5c4bfb4-6d32-4a46-a891-39b98114bf95" />

<img width="903" height="487" alt="image" src="https://github.com/user-attachments/assets/22a44383-5bff-4af6-bb7a-93394514fbe8" />

<img width="904" height="650" alt="image" src="https://github.com/user-attachments/assets/b9f42e8d-9ed8-4ae9-9164-195fc7491bbc" />

<img width="908" height="620" alt="image" src="https://github.com/user-attachments/assets/5c36a6c0-e54f-46bd-b9b2-2d27e19b709a" />




```

---
