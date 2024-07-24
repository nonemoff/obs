---
дневник:
---
<%*
// Get note`s date
const currentDayStr = tp.file.title;

// Parse date
const temp = tp.file.title.split('.');
const day = parseInt(temp[0]);
const month = parseInt(temp[1]);
const year = parseInt(temp[2]);

// Set days
const currentDay = new Date(year, month - 1, day);
const previousDay = new Date(currentDay);
previousDay.setDate(currentDay.getDate() - 1);
const nextDay = new Date(currentDay);
nextDay.setDate(currentDay.getDate() + 1);

// Format dates into strings
let previousDayYear = previousDay.getFullYear();
let previousDayMonth = String(previousDay.getMonth() + 1).padStart(2, '0');
let previousDayDate = String(previousDay.getDate()).padStart(2, '0');
let previousDayStr = `${previousDayDate}.${previousDayMonth}.${previousDayYear}`;

const nextDayYear = nextDay.getFullYear();
const nextDayMonth = String(nextDay.getMonth() + 1).padStart(2, '0');
const nextDayDate = String(nextDay.getDate()).padStart(2, '0');
const nextDayStr = `${nextDayDate}.${nextDayMonth}.${nextDayYear}`;

// Compute first Monday of the month
const firstMonday = new Date(year, month - 1, 1);
while (firstMonday.getDay() !== 1) {
    firstMonday.setDate(firstMonday.getDate() + 1);
}

// Compute current week start and end dates
const weekStart = new Date(currentDay);
weekStart.setDate(currentDay.getDate() - (currentDay.getDay() === 0 ? 6 : currentDay.getDay() - 1));
const weekEnd = new Date(weekStart);
weekEnd.setDate(weekStart.getDate() + 6);

// Format weeks into strings
let weekYear = weekStart.getFullYear();
let weekMonth = String(weekStart.getMonth() + 1).padStart(2, '0');
let weekStartStr = `${String(weekStart.getDate()).padStart(2, '0')}.${String(weekStart.getMonth() + 1).padStart(2, '0')}.${weekStart.getFullYear()}`;
let weekEndStr = `${String(weekEnd.getDate()).padStart(2, '0')}.${String(weekEnd.getMonth() + 1).padStart(2, '0')}.${weekEnd.getFullYear()}`;
let weekFileName = `${weekStartStr}-${weekEndStr}`;

// Adjust previous day link if it belongs to the current week but transitions into the next month
let previousDayMonthFolder = `/${weekYear}/месяцы/${weekMonth}`;
if (previousDay < weekStart && previousDay > firstMonday) {
    previousDayYear = previousDay.getFullYear();
    previousDayMonth = String(previousDay.getMonth() + 1).padStart(2, '0');
    previousDayMonthFolder = `/${previousDayYear}/месяцы/${previousDayMonth}`;
} else if (previousDay < firstMonday && previousDay < weekStart) {
    if (previousDay.getMonth() === 0) { // January
        previousDayYear -= 1;
        previousDayMonth = '12'; // December
    } else {
        previousDayMonth = String(previousDay.getMonth()).padStart(2, '0');
    }
    previousDayMonthFolder = `/${previousDayYear}/месяцы/${previousDayMonth}`;
}

nextDayMonthFolder = `/${weekYear}/месяцы/${weekMonth}`
if (nextDay > weekEnd){
	nextDayMonthFolder = `/${nextDayYear}/месяцы/${nextDayMonth}`;
}

const monthStr = `${weekMonth}.${weekYear}`;
const yearStr = `${weekYear}`;
%>

[<< <% previousDayStr %>](<% previousDayMonthFolder %>/дни/<% previousDayStr %>.md) / [<% weekFileName %>](/<% weekYear %>/месяцы/<% weekMonth %>/недели/<% weekFileName %>.md) / [<% monthStr %>](/<% yearStr %>/месяцы/<% weekMonth %>/<% monthStr %>.md) / [<% yearStr %>](/<% yearStr %>/<% yearStr %>.md) / [<% nextDayStr %> >>](<% nextDayMonthFolder %>/дни/<% nextDayStr %>.md)

![[<% weekFileName %>#**Задачи на неделю**]]

## **Планы на день**

### Основные

- [ ] ---

### Дополнительные

- [ ] ---

## **Мысли**

### Мысль 1
