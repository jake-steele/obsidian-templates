<%*
const multiMsgSep = '---';

const clip = await navigator.clipboard.read();
if(clip[0] && clip[0].types.includes('text/html')) {
	const htmlBlob = await clip[0].getType('text/html');
	const html  = await (new Response(htmlBlob)).text();
	//tR += "```html\n"+html+"\n```\n\n";
	const parser = new DOMParser();
	const doc =  parser.parseFromString(html, "text/html"); 
	const messages = Array.from(doc.querySelectorAll('.c-message_kit__message'));
	let addString = '';

	messages.forEach((m) => {
		const sender = m.querySelector('.c-message__sender_button');
		if(sender) {
			addString += `\n[[${sender.innerText}]]`;
			const msg_time = m.querySelector('.c-timestamp');
			if(msg_time) {
				addString += ` - [${msg_time.ariaLabel}](${msg_time.href})`;
			}
			addString += `\n> [!quote]\n> `;
		} else {
			addString += `> \n> ${multiMsgSep}\n> \n> `;
		}
		
		const body = m.querySelector('.c-message_kit__blocks');
		if(body) {
			//console.log(body);
			first = body.innerHTML.match(/<pre .+>((.|\n)*\/?)<\/pre>/);
			if(first) {
				const first_txt = first[1];
				var formatted = '```<br/>\n';
				formatted += first_txt;
				formatted += '<br/>\n```';
				//console.log(first_txt);
				//console.log(formatted);
				var newtxt = `${tp.obsidian.htmlToMarkdown(body.innerHTML.replace(first_txt, formatted)).replace(/\n/g, '\n> ').replace(/> [\n]?>/, '>')}\n`;
				addString += newtxt;
			} else {
				addString += `${tp.obsidian.htmlToMarkdown(body.innerHTML).replace(/\n/g, '\n> ').replace(/> [\n]?>/, '>')}\n`;
			}
		}

		const text = m.querySelector('.c-message_kit__text');
		if(text) {
			addString += `${text.innerText.replace('\n', '\n> ')}\n`;
		}
	});
	
	tR += addString;
} else {
	tR = 'no html in clipboard';
}
%>