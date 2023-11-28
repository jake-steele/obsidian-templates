---
aliases: []
created: <% tp.file.creation_date("YYYY-MM-DD") %>
type: #periodic
status: 
tags: []
---
Tags:: #periodic

# THIS IS A WORKING COPY OF THE PERIODIC TEMPLATE THAT I AM WORKING ON

## New Template
<%*

// This is a template for creating new periodic notes
// It should be able to determine which type of note is being created from the file name
// It should then use this "noteType" to determine which template to use, etc.

// Initial setup
// File DIRS
const TS_ROOT = "2-Timestamps";
const DIR_FMT_YEAR = `[${TS_ROOT}/]YYYY`;
const DIR_FMT_MONTH = `${DIR_FMT_YEAR}[/]MM[ - ]MMM`;

// File NAMES
const FILENAME_YEAR = "YYYY";
const FILENAME_QUARTER = "YYYY[-Q]Q";
const FILENAME_WEEK = "YYYY[-W]ww";
const FILENAME_MONTH = "YYYY[-]MM[ ]MMM";
const FILENAME_DAY = "YYYY[-]MM[-]DD";

// File PATHS
const FILE_FMT_YEAR = `${DIR_FMT_YEAR}[/]${FILENAME_YEAR}`;
const FILE_FMT_QUARTER = `${DIR_FMT_YEAR}[/]${FILENAME_QUARTER}`;
const FILE_FMT_WEEK = `${DIR_FMT_YEAR}[/]${FILENAME_WEEK}`;
const FILE_FMT_MONTH = `${DIR_FMT_MONTH}[/]${FILENAME_MONTH}`;
const FILE_FMT_DAY = `${DIR_FMT_MONTH}[/]${FILENAME_DAY}`;

// Short templates to be appended to the end of the note
const TEMPLATES_DIR = "3-Permanent/39-Background/Templates/Notes/Template-Inserts";

const YEAR_TEMPLATE = "Yearly-Insert";
const QUARTER_TEMPLATE = "Quarterly-Insert";
const MONTH_TEMPLATE = "Monthly-Insert";
const WEEK_TEMPLATE = "Weekly-Insert";
const DAY_TEMPLATE = "Daily-Insert";

const TEMPLATE_PATHS = {
    "year": {
        "path": `${TEMPLATES_DIR}/${YEAR_TEMPLATE}`,
        "fileName": `${YEAR_TEMPLATE}`
    },
    "quarter": {
        "path": `${TEMPLATES_DIR}/${QUARTER_TEMPLATE}`,
        "fileName": `${QUARTER_TEMPLATE}`
    },
    "month": {
        "path": `${TEMPLATES_DIR}/${MONTH_TEMPLATE}`,
        "fileName": `${MONTH_TEMPLATE}`
    },
    "week": {
        "path": `${TEMPLATES_DIR}/${WEEK_TEMPLATE}`,
        "fileName": `${WEEK_TEMPLATE}`
    },
    "day": {
        "path": `${TEMPLATES_DIR}/${DAY_TEMPLATE}`,
        "fileName": `${DAY_TEMPLATE}`
    },
};

// Define a PeriodicNote class?
class PeriodicNote {
    constructor(fileName) {
        this.fileName = fileName;
        this.noteType = determineNoteType(fileName);
        this.moment = noteToMoment();
        this.path = notePath();
        this.link = linkToNote();
        this.parentDirs = setParentDirs();

        this.makeParentDirs();
    }

    // Methods
    determineNoteType() {
        /**
         * Determines the type of note based on the given file title.
         * 
         * @return {string} The type of note: "year", "quarter", "month", "week", or "day".
         */
    
        const yearPattern = /(\d{4})/;
        const quarterPattern = /(\d{4}-Q\d)/;
        const monthPattern = /(\d{4}-\d{2})/;
        const dayPattern = /(\d{4}-\d{2}-\d{2})/;
        switch (this.fileName) {
            case yearPattern: return "year";
            case quarterPattern: return "quarter";
            case monthPattern: return "month";
            case weekPattern: return "week";
            case dayPattern: return "day";
            default: return "day";
        }
    }

    noteToMoment() {
        switch (this.noteType) {
            case "year": return moment(this.fileName, FILE_FMT_YEAR);
            case "quarter": return moment(this.fileName, FILE_FMT_QUARTER);
            case "week": return moment(this.fileName, FILE_FMT_WEEK);
            case "month": return moment(this.fileName, FILE_FMT_MONTH);
            case "day": return moment(this.fileName, FILE_FMT_DAY);
            default: return moment(this.fileName, FILE_FMT_DAY);
        }
    }
    
    notePath() {
        /**
         * Returns the intended absolute note path for a given dayMoment and linkType.
         *
         * @return {string} The intended absolute note path.
         */
        
        let formatToUse = "";
        switch (this.noteType) {
            case "year": formatToUse = FILE_FMT_YEAR; break;
            case "quarter": formatToUse = FILE_FMT_QUARTER; break;
            case "month": formatToUse = FILE_FMT_MONTH; break;
            case "week": formatToUse = FILE_FMT_WEEK; break;
            case "day": formatToUse = FILE_FMT_DAY; break;
            default: formatToUse = FILE_FMT_DAY; break;
        }

        return this.moment.format(formatToUse);
    }

    linkToNote() {
        return `[[${this.path}|${this.fileName}]]`;
    }

    linkToRelative(changeType, changeAmount) {
        // Verify that changeType is valid
        if (!(changeType in ["years", "quarters", "months", "weeks", "days"])) {
            console.log("Invalid changeType: " + changeType);
            return;
        }

        let newMoment = this.moment;
        newMoment.add(changeAmount, changeType);
        switch (changeType) {
            case "years": return `[[${newMoment.format(FILE_FMT_YEAR)}|${newMoment.format(FILENAME_YEAR)}]]`;
            case "quarters": return `[[${newMoment.format(FILE_FMT_QUARTER)}|${newMoment.format(FILENAME_QUARTER)}]]`;
            case "months": return `[[${newMoment.format(FILE_FMT_MONTH)}|${newMoment.format(FILENAME_MONTH)}]]`;
            case "weeks": return `[[${newMoment.format(FILE_FMT_WEEK)}|${newMoment.format(FILENAME_WEEK)}]]`;
            case "days": return `[[${newMoment.format(FILE_FMT_DAY)}|${newMoment.format(FILENAME_DAY)}]]`;
            default: return `[[${newMoment.format(FILE_FMT_DAY)}|${newMoment.format(FILENAME_DAY)}]]`;
        }
    }

    setParentDirs() {
        let parentDirsArr = [];
        parentDirsArr.push(this.moment.format(DIR_FMT_YEAR));
        if (this.noteType in ["month", "day"]) {
            parentDirsArr.push(this.moment.format(DIR_FMT_MONTH));
        }

        return parentDirsArr;
    }

    makeParentDirs() {
        async function makeDir(path) {
            try {
                await this.app.vault.createFolder(path);
                console.log("Created dir: " + path);
            } catch(error) {
                console.log("Failed to create dir: " + path);
                console.log(error);
            };
        }

        for (dir in this.parentDirs) {
            makeDir(dir);
        }

        return;
    }

    async tryMove() {
        await tp.file.move(this.path);
        console.log("Moved note to: " + this.path);
        
        return;
    }

    
    fillTemplate() {
        function breadcrumbsString() {
            /**
             * Returns a string containing links to the year, quarter, month, week, and day.
             *
             * @return {string} description of return value
             */
            let quarterNotes = ["quarter", "month", "week", "day"];
            let monthNotes = ["month", "week", "day"];
            let weekNotes = ["week", "day"];
            
            let yearLink = linkToNote(this.moment, "year");
            let breadString = `${yearLink}`;
            if (quarterNotes.includes(this.noteType)) {
                let quarterLink = linkToNote(this.moment, "quarter");
                breadString += ` > ${quarterLink}`;
            }
            if (monthNotes.includes(this.noteType)) {
                let monthLink = linkToNote(this.moment, "month");
                breadString += ` > ${monthLink}`;
            }
            if (weekNotes.includes(this.noteType)) {
                let weekLink = linkToNote(this.moment, "week");
                breadString += ` > ${weekLink}`;
            }
            if (this.noteType == "day") {
                let dayLink = linkToNote(this.moment, "day");
                breadString += ` > ${dayLink}`;
            }
    
            return breadString;
        }
    
        function getTitleFormat() {
            switch (this.noteType) {
                case "year": return "YYYY[ - Year Summary]";
                case "quarter": return "YYYY[-Q]Q[ - Quarter Summary]";
                case "month": return "YYYY[-]MM[ ]MMM[ - Month Summary]";
                case "week": return "YYYY[-W]ww[ - Week Summary]";
                case "day": return "MMM[ ]Do[, ]YYYY[ (]dddd[)]";
                default: return "MMM[ ]Do[, ]YYYY[ (]dddd[)]";
            }
        }
    
        function getNavLinks() {
            // Maybe make a subfunction that determines "prev" and "next" to be referenced below?
            if (this.noteType == "day") {
                // Grab yesterday & tomorrow, change if beginning/end of week
                let yday = this.moment.subtract(1, 'days');
                let tmrw = this.moment.add(1, 'days');
                switch (this.moment.weekday()) {
                    case 1: yday = this.moment.subtract(3, 'days'); break;
                    case 5: tmrw = this.moment.add(3, 'days'); break;
                };
            }

            let navLinks = "";
            switch (this.noteType) {
                case "year": navLinks = `<< ${linkToRelative("years", -1)}  |  ${this.moment.format("YYYY")}  |  ${linkToRelative("years", 1)} >>`; break;
                case "quarter": navLinks = `<< ${linkToRelative("quarters", -1)}  |  ${this.moment.format("YYYY[-]Q")}${this.moment.quarter()}  |  ${linkToRelative("quarters", 1)} >>`; break;
                case "month": navLinks = `<< ${linkToRelative("months", -1)}  |  ${this.moment.format("YYYY[-]MM[ ]MMM")}  |  ${linkToRelative("months", 1)} >>`; break;
                case "week": navLinks = `<< ${linkToRelative("weeks", -1)}  |  ${this.moment.format("YYYY[-W]ww")}  |  ${linkToRelative("weeks", 1)} >>`; break;
                case "day": navLinks = `<< ${linkToNote(yday, "day")}  |  ${this.moment.format("YYYY[-]MM[-]DD")}  |  ${linkToNote(tmrw, "day")} >>`; break;
                default: navLinks = `<< ${linkToNote(yday, "day")}  |  ${this.moment.format("YYYY[-]MM[-]DD")}  |  ${linkToNote(tmrw, "day")} >>`; break;
            }
    
            return navLinks;
        }

        tR += `${breadcrumbsString()}\n`;
        tR += `# ${this.moment.format(getTitleFormat())}\n`;
        tR += `${getNavLinks()}\n\n`;
        
        // Build the note out
        switch (this.noteType) {
            case "year": tp.file.include(`[[${TEMPLATE_PATHS.year.path}]]`); break;
            case "quarter": tp.file.include(`[[${TEMPLATE_PATHS.quarter.path}]]`); break;
            case "month": tp.file.include(`[[${TEMPLATE_PATHS.month.path}]]`); break;
            case "week": tp.file.include(`[[${TEMPLATE_PATHS.week.path}]]`); break;
            case "day": tp.file.include(`[[${TEMPLATE_PATHS.day.path}]]`); break;
            default: tp.file.include(`[[${TEMPLATE_PATHS.day.path}]]`); break;
        }

        return;
    }
}

let thisNote = new PeriodicNote(tp.file.title);
thisNote.fillTemplate();
thisNote.tryMove();

%>
