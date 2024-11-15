<%*
function createYearStructure(year, pathPrefix) {
	const months = [
		["Январь", "January"], ["Февраль", "February"], ["Март", "March"], ["Апрель", "April"], ["Май", "May"], ["Июнь", "June"],
		["Июль", "July"], ["Август", "August"], ["Сентябрь", "September"], ["Октябрь", "October"], ["Ноябрь", "November"], ["Декабрь", "December"]
	];
	const yearFolder = `${pathPrefix}/${year}`;
	const monthsStructure = {};

	months.forEach((month, index) => {
		const monthNumber = String(index + 1).padStart(2, "0");
		monthsStructure[month[1].toLowerCase()] = {
			name: month[0],
			path: `${yearFolder}/months/${monthNumber}/${monthNumber}.${year}`,
			folder: `${yearFolder}/months/${monthNumber}`
		};
	});

	return {
		name: String(year),
		path: `${yearFolder}/${year}`,
		folder: yearFolder,
		months: monthsStructure
	};
}

const PathPrefix = '/00-diary'
const currentYear = parseInt(tp.file.title);

const years = {
	previous: createYearStructure(currentYear - 1, PathPrefix),
	current: createYearStructure(currentYear, PathPrefix),
	next: createYearStructure(currentYear + 1, PathPrefix)
};
%>
[<< <% years.previous.name %>](<% years.previous.path %>) / [<% years.next.name %> >>](<% years.next.path %>)

## Месяцы

| <div style="width:100px">Зима</div> | <div style="width:100px">Весна</div> | <div style="width:100px">Лето</div> | <div style="width:100px">Осень</div> | <div style="width:100px">Зима</div> |
| ----------------------------------- | ------------------------------------ | ----------------------------------- | ------------------------------------ | ----------------------------------- |
| [<% years.current.months.january.name %>](<% years.current.months.january.path %>)<br> | [<% years.current.months.march.name %>](<% years.current.months.march.path %>)<br> | [<% years.current.months.june.name %>](<% years.current.months.june.path %>)<br> | [<% years.current.months.september.name %>](<% years.current.months.september.path %>)<br> | [<% years.current.months.december.name %>](<% years.current.months.december.path %>) |
| [<% years.current.months.february.name %>](<% years.current.months.february.path %>)<br> | [<% years.current.months.april.name %>](<% years.current.months.april.path %>)<br> | [<% years.current.months.july.name %>](<% years.current.months.july.path %>)<br> | [<% years.current.months.october.name %>](<% years.current.months.october.path %>)<br> | |
| | [<% years.current.months.may.name %>](<% years.current.months.may.path %>)<br> | [<% years.current.months.august.name %>](<% years.current.months.august.path %>)<br> | [<% years.current.months.november.name %>](<% years.current.months.november.path %>)<br> | |

## Цели

- [ ] ...