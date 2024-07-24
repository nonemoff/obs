<%*
const year = parseInt(tp.file.title.split('.')[1]);
const month = parseInt(tp.file.title.split('.')[0]);
const daysInMonth = new Date(year, month, 0).getDate();
const createDate = (day, month, year) => new Date(year, month - 1, day);
const isMonday = (date) => date.getDay() === 1;

const previousMonth = month === 1 ? 12 : month - 1;
const previousYear = month === 1 ? year - 1 : year;
const nextMonth = month === 12 ? 1 : month + 1;
const nextYear = month === 12 ? year + 1 : year;

const createNote = async (templateName, fileName, folderPath) => {
    const template = await tp.file.find_tfile(templateName);
    await tp.file.create_new(template, fileName, false, folderPath);
};

const monthFolder = `${year}/месяцы/${month.toString().padStart(2, '0')}`;
let firstMonday;
for (let day = 1; day <= daysInMonth; day++) {
    const date = createDate(day, month, year);
    if (isMonday(date)) {
        firstMonday = day;
        break;
    }
}

let startOfWeek = firstMonday;
let weekIndex = 1;
let weekLinks = "";
while (startOfWeek <= daysInMonth) {
    let endOfWeek = startOfWeek + 6;
    let endMonth = month;
    let endYear = year;

    if (endOfWeek > daysInMonth) {
        endOfWeek -= daysInMonth;
        endMonth = month + 1;
        if (endMonth > 12) {
            endMonth = 1;
            endYear++;
        }
    }

    const startStr = `${String(startOfWeek).padStart(2, '0')}.${month.toString().padStart(2, '0')}.${year}`;
    const endStr = `${String(endOfWeek).padStart(2, '0')}.${endMonth.toString().padStart(2, '0')}.${endYear}`;

    const weekFileName = `${startStr}-${endStr}`;
    const weekFolder = `${monthFolder}/недели`;
    await createNote("templates/week.md", weekFileName, weekFolder);

    weekLinks += `${weekIndex}. [[${weekFileName}]]\n`;
    weekIndex++;

    startOfWeek += 7;
}
%>

[<< <% previousMonth.toString().padStart(2, '0') %>.<% previousYear %>](/<% previousYear %>/месяцы/<% previousMonth.toString().padStart(2, '0') %>/<% previousMonth.toString().padStart(2, '0') %>.<% previousYear %>.md) / [<% year %>](/<% year %>/<% year %>.md) / [<% nextMonth.toString().padStart(2, '0') %>.<% nextYear %> >>](/<% nextYear %>/месяцы/<% nextMonth.toString().padStart(2, '0') %>/<% nextMonth.toString().padStart(2, '0') %>.<% nextYear %>.md)

## **Недели**

<% weekLinks %>

![[/<%year%>/<%year%>#Трекеры привычек]]

![[/<%year%>/<%year%>#Цели на год]]

## **Цели на месяц**

- [ ] Цель N

## **Мотивация**
### Цель N

Почему я ставлю себе такую цель на этот месяц

## **Итоги месяца**

### Мысль N