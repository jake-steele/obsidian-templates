---
aliases: []
created: <% tp.file.creation_date("YYYY-MM-DD") %>
type: #daily-note
status: #needs-review
tags: []
---
Tags:: #daily-note

# <% moment(tp.file.title).format("dddd - MMM Do, YYYY") %>
<%*
// Move this note into the right "Timestamps / Year / Month" directory
const fileTitle = tp.file.title;
const folderPath = "/2-Timestamps/";
const yearPath = folderPath + moment(fileTitle).format("YYYY/");
const monthPath = yearPath + moment(fileTitle).format("MM - MMM/");
const finalPath = monthPath + fileTitle;

const pathArray = [yearPath, monthPath];
try {
    await tp.file.move(finalPath);
} catch(error) {
    console.log(error);
    for (const idx in pathArray) {
        try {
            await this.app.vault.createFolder(pathArray[idx]);
            console.log("Created: " + pathArray[idx]);
        } catch(error2) {
            console.log("Failed to create: " + pathArray[idx]);
            console.log(error2);
        };
    };
    await tp.file.move(finalPath);
};
console.log("Final path: " + tp.file.path(true))

// Add Month / Week note links here:
// tR += "[[2023-10-Oct|10 - October]]  |    |  [[2023-10-Wk3|Week 3]]"
%>
<%*
const TS_ROOT = "/2-Timestamps";
let todayMoment = moment(tp.file.title);

// Grab yesterday & tomorrow, change if beginning/end of week
let yday = todayMoment.subtract(1, 'days');
let tmrw = todayMoment.add(1, 'days');
switch (todayMoment.weekday()) {
    case 1: yday = todayMoment.subtract(3, 'days'); break;
	case 5: tmrw = todayMoment.add(3, 'days'); break;
};

function linkTo(filePath) {
    return `[[${filePath}]]`
};

function notePath(dayMoment, linkType) {
    const FMT_DEFAULT = "YYYY[-]MM[-]DD";
    const PATH_FMT_YEAR = `[${TS_ROOT}/]YYYY`;
    const PATH_FMT_MONTH = `${PATH_FMT_YEAR}[/]MM[ - ]MMM`;
    
    const FILE_FMT_YEAR = `${PATH_FMT_YEAR}[/]YYYY`;
    const FILE_FMT_QUARTER = `${PATH_FMT_YEAR}[/]YYYY[-Q]Q`;
    const FILE_FMT_WEEK = `${PATH_FMT_YEAR}[/]YYYY[-W]ww`;
    const FILE_FMT_MONTH = `${PATH_FMT_MONTH}[/]YYYY[-]MM`;
    const FILE_FMT_DAY = `${PATH_FMT_MONTH}[/]${FMT_DEFAULT}`;

    switch (linkType) {
        case "year": return linkTo(dayMoment.format(FILE_FMT_YEAR));
        case "quarter": return linkTo(dayMoment.format(FILE_FMT_QUARTER));
        case "month": return linkTo(dayMoment.format(FILE_FMT_MONTH));
        case "week": return linkTo(dayMoment.format(FILE_FMT_WEEK));
        case "day": return linkTo(dayMoment.format(FILE_FMT_DAY));
        default: return linkTo(dayMoment.format(FILE_FMT_DAY));
    }
}

function linkToNote(dayMoment, linkType) {
    return linkTo(notePath(dayMoment, linkType));
}

function breadcrumbsString(dayMoment) {
    // Returns a string containing links to the year, quarter, month, week, and day
    let yearLink = linkToNote(dayMoment, "year");
    let quarterLink = linkToNote(dayMoment, "quarter");
    let monthLink = linkToNote(dayMoment, "month");
    let weekLink = linkToNote(dayMoment, "week");
    let dayLink = linkToNote(dayMoment, "day");
    
    let breadString = `${yearLink} > ${quarterLink} > ${monthLink} > ${weekLink} > ${dayLink}`;
    return breadString;
}

tR += `<< ${linkToNote(yday, "day")}  |  ${breadcrumbsString(todayMoment)}  |  ${linkToNote(tmrw, "day")} >>`;
%>

|                      Created                      |                      Last Modified                      |
|:-------------------------------------------------:|:-------------------------------------------------------:|
| <% tp.file.creation_date("YYYY-MM-DD @ HH:mm") %> | <%+ tp.file.last_modified_date("YYYY-MM-DD @ HH:mm") %> |

---
