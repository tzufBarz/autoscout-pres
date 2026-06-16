---
theme: default
title: AutoScout
transition: fade
routerMode: hash
---

---
layout: cover

background: ./background.png
class: rtl cover-slide
transition: slide-left
---

<div class="cover-block">

# <span class="en">AutoScout</span>&rlm;

<div class="subtitle">
מערכת מעקב אוטומטית לרובוטים בתחרויות <bdi>FRC</bdi>&rlm;
</div>

</div>

<!--
AutoScout היא מערכת שמקבלת סרטון מקצה FRC ומספר מקצה, ומחזירה מסלולים של רובוטים שמשויכים לקבוצות.
-->

---
layout: default
class: rtl demo-slide
transition: slide-up
---

<div class="center-block">

# הדגמה

<div class="subtitle">
מתחילים עיבוד על קליפ קצר
</div>

</div>

<!--
אני מתחיל את העיבוד כבר עכשיו, כי הוא עובר על הרבה פריימים ולוקח זמן על הלפטופ.
בזמן שהוא רץ, אני אחזור למצגת ואסביר מה המערכת פותרת ואיך היא עובדת.
כאן לצאת מהמצגת, להעלות קליפ קצר, לבחור מספר מקצה, להתחיל עיבוד, ולחזור למצגת.
-->

---
layout: default
class: rtl
transition: slide-left
---

# הבעיה

<div class="simple-list">

<div>סקאוטינג ידני דורש הרבה כוח אדם.</div>

<div>קשה לעקוב אחרי שישה רובוטים במקביל.</div>

<div>חסרים מסלולים מלאים לאורך זמן.</div>

</div>

<!--
בסקאוטינג ידני קשה לעקוב אחרי שישה רובוטים במקביל, במיוחד כשיש תנועה מהירה וחסימות.
בנוסף, בדרך כלל מקבלים אירועים או הערכות, אבל לא מסלול מלא של כל רובוט לאורך זמן.
-->

---
layout: default
class: rtl pipeline-slide
transition: slide-left
---

# הפתרון

<div class="pipeline-box">

```mermaid

%%{init: {
  "theme": "base",
  "themeCSS": ".node rect { rx: 6px; ry: 6px; } .flowchart-link { stroke: #8f8f8f !important; stroke-width: 1.5px !important; } marker path { fill: #8f8f8f !important; stroke: #8f8f8f !important; }"
}}%%
flowchart RL
    subgraph inputs[" "]
      direction TB
      video["סרטון מקצה"]
      match["מספר מקצה"]
    end

    video --> robots[":YOLO26<br/>זיהוי רובוטים"]
    robots --> track[":ByteTrack<br/>מעקב"]
    track --> voting["שיוך<br/>קבוצה"]
    voting --> smooth["החלקת<br/>מסלולים"]
    smooth --> replay[":Replay<br/>באפליקציה"]

    track --> digits[":YOLO26<br/>זיהוי ספרות"]
    digits --> voting

    match --> schedule["קבוצות<br/>המקצה"]
    schedule --> voting

    classDef input fill:#1f2937,stroke:#4b5563,stroke-width:1px,color:#f3f4f6
    classDef model fill:#163326,stroke:#3f6f55,stroke-width:1px,color:#f3f4f6
    classDef tracking fill:#2a421b,stroke:#4f7d4f,stroke-width:1px,color:#f3f4f6
    classDef logic fill:#3a2a17,stroke:#8a6a3a,stroke-width:1px,color:#f3f4f6
    classDef data fill:#30301c,stroke:#77704a,stroke-width:1px,color:#f3f4f6
    classDef output fill:#282238,stroke:#5f557c,stroke-width:1px,color:#f3f4f6

    class video,match input
    class robots,digits model
    class track tracking
    class voting,smooth logic
    class schedule data
    class replay output

    style inputs fill:transparent,stroke:transparent,color:transparent

```

</div>

<!--
הנקודה המרכזית היא ש-track ID הוא לא team ID.
ByteTrack יכול להגיד לי שזה אותו אובייקט לאורך כמה פריימים, אבל הוא לא יודע איזו קבוצה זו.
בשביל להפוך עקבה זמנית לקבוצה אמיתית, צריך לזהות ספרות, להשוות לקבוצות האפשריות במקצה, ולצבור החלטות לאורך זמן.
-->

---
layout: default
class: rtl
transition: slide-left
---

# דאטה ומודלים

<div class="two-columns">

<div>

### רובוטים ובריתות

1000+ תמונות קיימות.

זיהוי רובוטים,

סיווג בריתות,

מסגרות גדולות יחסית.
</div>

<div>

### ספרות

כ-500 תמונות מתויגות ידנית.

זיהוי ספרות בודדות,

חיתוכי באמפרים,

הכללה לקבוצות שלא הופיעו באימון.
</div>

</div>

<!--
אימנתי שני מודלי YOLO שונים: אחד לזיהוי רובוטים ובריתות, ואחד לזיהוי ספרות.
את הספרות זיהיתי כספרות בודדות ולא כמספרי קבוצות שלמים, כדי שהמערכת תוכל להתמודד גם עם קבוצות שלא הופיעו באימון.
-->

---
layout: default
class: rtl
transition: slide-left
---

# אימון והערכה

<div class="three-columns compact">

<div>

### שני מודלים

<span class="en">YOLO</span>&rlm; לזיהוי רובוטים<br/>
<span class="en">YOLO</span>&rlm; לזיהוי ספרות

</div>

<div>

### מדדים

<span class="en">Precision / Recall / mAP</span>&rlm;
</div>

<div>

### בדיקה מלאה

<span class="en">End-to-end</span>&rlm;<br/>
תיקוף ידני של שיוך לקבוצות
</div>

</div>

<!--
הערכתי את המודלים במדדים רגילים של זיהוי אובייקטים.
אבל בפרויקט הזה חשוב גם לבדוק האם המערכת המלאה מצליחה להפיק מסלולים משויכים לקבוצות.
-->

---
layout: default
class: rtl
transition: slide-left
---

# התוצאות

<div class="three-columns compact">

<div>

### זיהוי רובוטים

mAP@50 = 0.97
Precision = 0.93
Recall = 0.94

</div>

<div>

### זיהוי ספרות

mAP@50 = 0.85
Precision = 0.75
Recall = 0.78

</div>

<div>

### שיוך end-to-end

75% שיוך נכון
273 מופעי רובוט
3 מקצים מתחרויות שונות

</div>

</div>

<!--
מודל הרובוטים יציב יותר, כי הרובוטים גדולים וברורים יחסית.
מודל הספרות קשה יותר, כי הספרות קטנות, מטושטשות ולעיתים מוסתרות.
לכן לא מספיק להסתכל רק על מדד של מודל בודד, אלא גם על התוצאה הסופית של המערכת.
-->

---
layout: default
class: rtl
---

# מגבלות

<div class="three-columns compact">

<div>

### הסתרות

רובוטים מסתירים אחד את השני.
</div>

<div>

### טשטוש

תנועה מהירה ואיכות צילום.
</div>

<div>

### איבוד מעקב

קשה לזהות מחדש אחרי איבוד עקבה.
</div>

</div>

<!--
המערכת לא מושלמת.
הכשלים העיקריים הם חסימות, טשטוש, וזיהוי מחדש אחרי איבוד track.
אם הייתי ממשיך את הפרויקט, הייתי משפר Re-ID וזיהוי ספרות בתנאי צילום קשים.
-->

---
layout: default
class: rtl demo-slide
transition: slide-down
---

<div class="center-block">

# חזרה לדמו

<div class="subtitle">
<bdi>Replay</bdi>&rlm; עם מסלולים משויכים
</div>

</div>

<!--
עכשיו נחזור למערכת ונראה את הפלט הסופי.
כאן רואים שהמערכת לא רק מזהה רובוטים בפריים בודד, אלא מפיקה מסלולים לאורך זמן ומשייכת אותם לקבוצות.
זה המידע שבאמת שימושי לסקאוטינג.
-->

---
layout: default
class: rtl
---

# סיכום

<div class="three-columns compact final">

<div>

### <span class="en">Computer Vision</span>&rlm;

זיהוי רובוטים וספרות.
</div>

<div>

### <span class="en">Tracking</span>&rlm;

מעקב לאורך זמן.
</div>

<div>

### <span class="en">System Design</span>&rlm;

מערכת מקצה לקצה.
</div>

</div>

<!--
הדבר המרכזי שלמדתי הוא שהאתגר לא היה רק לאמן מודל, אלא לחבר כמה רכיבים למערכת אחת:
זיהוי רובוטים, מעקב, זיהוי ספרות, לוגיקת שיוך, החלקה והצגה באפליקציה.
-->
