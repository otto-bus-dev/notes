
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

function displayFolder(folder, level, path){
	let folder_rows = folder.rows.filter(
			page => page.path.contains(path)
		)
	let folder_path = path;
			if (path == ""){
				folder_path = "none";
			}else{
				folder_path = path;
			}
	for (let sub_folder of groupBy(
		folder_rows, 
		page => page['level_' + level ]
	)) {
		if( sub_folder.key !== "" && sub_folder.key !== null && sub_folder.key !== 'undefined'){
			let sub_folder_path = path;
			if (path == ""){
				sub_folder_path = sub_folder.key;
			}else{
				sub_folder_path = path + "/" + sub_folder.key;
			}
			dv.header(3+level, sub_folder.key, {cls: "folder level_" + level, attr: { id: sub_folder_path  ,  folder: folder_path }});
			
			//dv.el("div", sub_folder_path )
			displayFolder(sub_folder,level + 1, sub_folder_path)
			
		}
	}
	for (let folder_content of folder_rows.filter(page => page.path==path)) {
		dv.el("div", folder_content.file.link, { cls: "level_" + level, attr: { folder: folder_path } })
	}
	/*
	for (let folder_content of folder_rows) {
		//if( folder_content.page['level_' + level] == null){
			dv.el("div", folder_content.file.link)
		//}
	}*/
}

let pages = [];
for (let page of dv.pages()) {
	let i = 0;
	let new_page = {
		path:page.file.folder,
		file :page.file
	}
	for (let path_part of new_page.path.split('/')){
		new_page["level_" + i] = path_part
		i++;
	}
	pages.push(new_page)
}

displayFolder({
	rows : pages
},0,"");


```
