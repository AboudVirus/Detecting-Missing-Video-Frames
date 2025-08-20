# Detecting Missing Video Frames

## Author
**Name:** Abdullah Abdul-Zahra Makawi  
**Phone:** 07807376771  

---

## Description (Arabic)

هذا المشروع يحتوي على دالة تقوم بتحليل أرقام الفريمات المستلمة من بث فيديو لكشف الفريمات المفقودة.  
الفريمات تكون مرقمة تصاعدياً ابتداءً من 1، لكن بسبب مشاكل في الشبكة أو البطء في الأجهزة قد يتم فقدان بعض الفريمات.  

### المهام التي ينفذها الكود:
1. **كشف الفجوات (gaps):** أي المديات المفقودة من الفريمات.
2. **تحديد أطول فجوة:** أكبر مدى يحتوي على أكبر عدد من الفريمات المفقودة.
3. **حساب العدد الكلي للفريمات المفقودة.**

### تفاصيل الحل:
- استخدام `Set` للتحقق السريع من وجود الفريمات.
- تحديد أقل وأعلى فريم (minFrame و maxFrame).
- المرور على جميع الفريمات بين الأصغر والأكبر، والكشف عن الفجوات.
- إرجاع النتيجة في شكل كائن يحتوي على:
  - قائمة الفجوات.
  - أطول فجوة.
  - العدد الكلي للفريمات المفقودة.

---

## Example (JavaScript)

```javascript
function findMissingRanges(frames) {
  if (!frames?.length) {
    return {
      gaps: [],
      longest_gap: null,
      missing_count: 0
    };
   }

  const minFrame = Math.min(...frames);
  const maxFrame = Math.max(...frames);
  const frameSet = new Set(frames);

  const gaps = [];
  let longestGap = null;
  let missingCount = 0;

  for (let i = minFrame; i <= maxFrame; i++) {
    if (!frameSet.has(i)) {
      const start = i;
      while (i <= maxFrame && !frameSet.has(i)) i++;
      const end = i - 1;

      gaps.push([start, end]);

      const gapLength = end - start + 1;
      missingCount += gapLength;

      if (!longestGap || gapLength > (longestGap[1] - longestGap[0] + 1)) {
        longestGap = [start, end];
      }
    }
  }

  return {
     gaps,
     longest_gap: longestGap,
     missing_count: missingCount
  };
}

const frames = [1, 2, 3, 5, 6, 10, 11, 16];
console.log(findMissingRanges(frames));
```
---

## ✅ Expected Output

```js
{
  gaps: [ [ 4, 4 ], [ 7, 9 ], [ 12, 15 ] ],
  longest_gap: [ 12, 15 ],
  missing_count: 8
}
