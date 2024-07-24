<%*
const year = parseInt(tp.file.title);
const previousYear = year - 1;
const nextYear = year + 1;
const months = Array.from({length: 12}, (_, i) => String(i + 1).padStart(2, '0'));

const createNote = async (templateName, fileName, folderPath) => {
    const template = await tp.file.find_tfile(templateName);
    await tp.file.create_new(template, fileName, false, folderPath);
};

for (let month of months) {
    const monthFolder = `${year}/месяцы/${month}`;
    await createNote("templates/month.md", `${month}.${year}`, monthFolder);
}
%>

[<< <% previousYear %>](/<% previousYear %>/<% previousYear %>)  /  [<% nextYear %> >>](/<% nextYear %>/<% nextYear %>)
## ## **Месяцы**

| <div style="width:100px">Зима</div> | <div style="width:100px">Весна</div> | <div style="width:100px">Лето</div> | <div style="width:100px">Осень</div> | <div style="width:100px">Зима</div> |
| ----------------------------------- | ------------------------------------ | ----------------------------------- | ------------------------------------ | ----------------------------------- |
| [Январь](/<% year %>/месяцы/01/01.<% year %>.md)<br> | [Март](/<% year %>/месяцы/03/03.<% year %>.md)<br> | [Июнь](/<% year %>/месяцы/06/06.<% year %>.md)<br> | [Сентябрь](/<% year %>/месяцы/09/09.<% year %>.md)<br> | [Декабрь](/<% year %>/месяцы/12/12.<% year %>.md) |
| [Февраль](/<% year %>/месяцы/02/02.<% year %>.md)<br> | [Апрель](/<% year %>/месяцы/04/04.<% year %>.md)<br> | [Июль](/<% year %>/месяцы/07/07.<% year %>.md)<br> | [Октябрь](/<% year %>/месяцы/10/10.<% year %>.md)<br> | |
| | [Май](/<% year %>/месяцы/05/05.<% year %>.md)<br> | [Август](/<% year %>/месяцы/08/08.<% year %>.md)<br> | [Ноябрь](/<% year %>/месяцы/11/11.<% year %>.md)<br> | |

## Трекеры привычек

```dataviewjs

dv.span("**Ведение дневника**")
const calendarData = {
	year: <% year %>,
	colors: {
		blue:        ["#8cb9ff", "#69a3ff", "#428bff", "#1872ff", "#0058e2"],
		green:       ["#c6e48b", "#7bc96f", "#49af5d", "#2e8840", "#196127"],
		red:         ["#ff9e82", "#ff7b55", "#ff4d1a", "#e73400", "#bd2a00"],
		orange:      ["#ffa244", "#fd7f00", "#dd6f00", "#bf6000", "#9b4e00"],
		pink:        ["#ff96cb", "#ff70b8", "#ff3a9d", "#ee0077", "#c30062"],
		orangeToRed: ["#ffdf04", "#ffbe04", "#ff9a03", "#ff6d02", "#ff2c01"]
	},
	showCurrentDayBorder: true,
	defaultEntryIntensity: 0,
	intensityScaleStart: 0,
	intensityScaleEnd: 1,
	entries: [],
}

function convertDateFormat(dateStr) { 
	let parts = dateStr.split(".");
	let day = parts[0];
	let month = parts[1];
	let year = parts[2];
	let newDateStr = `${year}-${month}-${day}`;
	return newDateStr;
}

//DataviewJS loop
for (let page of dv.pages('"<% year %>"').where(p => p.дневник)) {
	//dv.span("<br>" + page.file.name) // uncomment for troubleshooting	
	calendarData.entries.push({
		date: convertDateFormat(page.file.name),     // (required) Format YYYY-MM-DD
		intensity: page.дневник, // (required) the data you want to track, will map color intensities automatically
		color: "green",          // (optional) Reference from *calendarData.colors*. If no color is supplied; colors[0] is used
	})
}

renderHeatmapCalendar(this.container, calendarData)

```

## **Цели на год**

- [ ] Цель N

## **Мотивация**

### Цель N

Почему я себе ставлю такую цель?