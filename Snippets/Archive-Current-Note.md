<%*
let currentFile = await this.app.workspace.getActiveFile();
var filePath = currentFile.path.toString();
filePath = filePath.split("/").slice(0,-1).join("/");
filePath += '/archive/';

if (!await app.vault.exists(filePath)) {
    await app.vault.createFolder(filePath);
}

filePath += tp.file.title + "_" + moment().format('YYYY-MM-DD');
await app.vault.copy(currentFile, filePath);
%>