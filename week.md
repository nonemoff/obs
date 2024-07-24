<%*
const week = tp.file.title.split('-');
const start = week[0];
const end = week[1];

const startDay = parseInt(start.split('.')[0]);
const startMonth = parseInt(start.split('.')[1]);
const startYear = parseInt(start.split('.')[2]);

const endDay = parseInt(end.split('.')[0]);
const endMonth = parseInt(end.split('.')[1]);
const endYear = parseInt(end.split('.')[2]);

const createNote = async (templateName, fileName, folderPath) => {
    const template = await tp.file.find_tfile(templateName);
    await tp.file.create_new(template, fileName, false, folderPath);
};

const days = [];
const daysInStartMonth = new Date(startYear, startMonth, 0).getDate();

if (startYear === endYear && startMonth === endMonth) {
    for (let day = startDay; day <= endDay; day++) {
        days.push({ day, month: startMonth, year: startYear });
    }
} else {
    for (let day = startDay; day <= daysInStartMonth; day++) {
        days.push({ day, month: startMonth, year: startYear });
    }
    for (let day = 1; day <= endDay; day++) {
        days.push({ day, month: endMonth, year: endYear });
    }
}

const monthFolder = `${startYear}/месяцы/${String(startMonth).padStart(2, '0')}`;
for (let date of days) {
    const dayStr = String(date.day).padStart(2, '0');
    const monthStr = String(date.month).padStart(2, '0');
    const dayFileName = `${dayStr}.${monthStr}.${date.year}`;
    const dayFolder = `${monthFolder}/дни`;
    await createNote("templates/day.md", dayFileName, dayFolder);
}

// Compute the previous and next weeks
const previousWeekStart = new Date(startYear, startMonth - 1, startDay - 7);
const nextWeekStart = new Date(startYear, startMonth - 1, startDay + 7);

const previousWeekEnd = new Date(endYear, endMonth - 1, endDay - 7);
const nextWeekEnd = new Date(endYear, endMonth - 1, endDay + 7);

const previousWeekStartStr = `${String(previousWeekStart.getDate()).padStart(2, '0')}.${String(previousWeekStart.getMonth() + 1).padStart(2, '0')}.${previousWeekStart.getFullYear()}`;
const previousWeekEndStr = `${String(previousWeekEnd.getDate()).padStart(2, '0')}.${String(previousWeekEnd.getMonth() + 1).padStart(2, '0')}.${previousWeekEnd.getFullYear()}`;

const nextWeekStartStr = `${String(nextWeekStart.getDate()).padStart(2, '0')}.${String(nextWeekStart.getMonth() + 1).padStart(2, '0')}.${nextWeekStart.getFullYear()}`;
const nextWeekEndStr = `${String(nextWeekEnd.getDate()).padStart(2, '0')}.${String(nextWeekEnd.getMonth() + 1).padStart(2, '0')}.${nextWeekEnd.getFullYear()}`;

const currentMonthStr = `${String(startMonth).padStart(2, '0')}.${startYear}`;
const currentYearStr = `${startYear}`;

// Generate the days of the week table
const daysOfWeek = ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"];
let dayIndex = 0;
let weekTable = "|";
let dateLinks = "|";

for (let date of days) { 
	weekTable += `${daysOfWeek[dayIndex % 7]} |`;
	dateLinks += `[[${String(date.day).padStart(2, '0')}.${String(date.month).padStart(2, '0')}.${date.year}]] |`;
	dayIndex++;
}

// Fill in the remaining cells if the week is not complete
while (dayIndex % 7 !== 0) {
    weekTable += " |";
    dateLinks += " |";
    dayIndex++;
}

weekTable += "\n";
dateLinks = dateLinks.slice(0, -2) + "\n"; // Remove the last extra " |" and add a newline

%>

[<< <% previousWeekStartStr %> - <% previousWeekEndStr %>](/<% previousWeekStart.getFullYear() %>/месяцы/<% String(previousWeekStart.getMonth() + 1).padStart(2, '0') %>/недели/<% previousWeekStartStr %>-<% previousWeekEndStr %>.md) / [<% currentMonthStr %>](/<% currentYearStr %>/месяцы/<% String(startMonth).padStart(2, '0') %>/<% String(startMonth).padStart(2, '0') %>.<% currentYearStr %>.md) / [<% currentYearStr %>](/<% currentYearStr %>/<% currentYearStr %>.md) / [<% nextWeekStartStr %> - <% nextWeekEndStr %> >>](/<% nextWeekStart.getFullYear() %>/месяцы/<% String(nextWeekStart.getMonth() + 1).padStart(2, '0') %>/недели/<% nextWeekStartStr %>-<% nextWeekEndStr %>.md)

![[/<% currentYearStr %>/<% currentYearStr %>.md#Трекеры привычек]]
![[/<% currentYearStr %>/месяцы/<% String(startMonth).padStart(2, '0') %>/<% String(startMonth).padStart(2, '0') %>.<% currentYearStr %>.md#Цели на месяц]]
## **Дни недели**

| Пн | Вт | Ср | Чт | Пт | Сб | Вс |
| --- | --- | --- | --- | --- | --- | --- |
<% dateLinks %>

## **Задачи на неделю**

- [ ] --

## **Мотивация**

### Цель 1

Буквы

## **Итоги недели**

### Мысль N

смыслы в буквах