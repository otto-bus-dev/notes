
```dataviewjs
dv.list()
```
```dataviewjs
for (let group of dv.pages("#book").where(p => p["time-read"].year == 2021).groupBy(p => p.genre)) {
	dv.header(3, group.key);
	dv.table(["Name", "Time Read", "Rating"],
		group.rows
			.sort(k => k.rating, 'desc')
			.map(k => [k.file.link, k["time-read"], k.rating]))
}
```
```dataviewjs
function groupBy(array, fn) {
    let groups = {};
    for (let item of array) {
        let key = fn(item) ?? 'undefined';
        if (!groups[key]) groups[key] = [];
        groups[key].push(item);
    }
    return Object.entries(groups).map(([key, rows]) => ({key, rows}));
}

let hierachied_pages = [];
for (let page of dv.pages()) {
	let i = 0;
	let new_page = {
		path:page.file.folder,
		level_0:null,
		level_1:null,
		level_2:null,
		file : page.file
	}
	for (let path_part of new_page.path.split('/')){
		new_page["level_" + i] = path_part
		i++;
	}
	hierachied_pages.push(new_page)
}
for (let folder of groupBy(hierachied_pages, page => page.level_0)) {
	dv.header(3, folder.key);
	for (let sub_folder of groupBy(folder.rows, page => page.level_1)) {
		if( sub_folder.key !== null && sub_folder.key !== 'undefined'){
			dv.header(4, sub_folder.key);
			for (let sub_folder_content of sub_folder.rows) {
					dv.el("div", sub_folder_content.file.link)
	
			}
		}
	}
	for (let folder_content of folder.rows) {
	
		if( folder_content.level_1 == null){
			dv.el("div", folder_content.file.link)
		}
	}
}

```
