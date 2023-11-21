<%*
const Seasons = {
	Summer: Symbol("summer"),
	Autumn: Symbol("autumn"),
	Winter: Symbol("winter"),
	Spring: Symbol("spring")
};

let season = Seasons.Spring;

console.log(Seasons);
console.log(season);

switch (season) {
	case Seasons.Summer:
	    console.log('the season is summer');
	    break;
	case Seasons.Winter:
	    console.log('the season is winter');
	    break;
	case Seasons.Spring:
	    console.log('the season is spring');
	    break;
	case Seasons.Autumn:
	    console.log('the season is autumn');
	    break;
	default:
		console.log('season not defined');
}
%>
