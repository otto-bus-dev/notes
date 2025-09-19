

```dataviewjs

const folder_replacement = '_21_';
const space_replacement = '_20_';
const pages = dv.pages().map(
	page => {
		return {
			name : page.file.name,
			link : page.file.link,
			folder : page.file.folder
		}
	}
);

function getId(path){
	return path.replace(/\//g,folder_replacement)
			.replace(/ /g,space_replacement);
}
const TYPE = {
	FILE:0,
	FOLDER:1
}
function getInfo(path,type){
	let folders = [];
	let folder_parts = path === ""?[]:path.split('/');
	let i = folder_parts.length - type;
	while(i>0){
		folders.push(folder_parts.slice(0,i)
			.join(folder_replacement)
			.replace(/ /g,space_replacement)
		);
		i--;
	}
	return {
		id : path.replace(/\//g,folder_replacement)
			.replace(/ /g,space_replacement),
		parent_id : folders.length > 0 ? folders[0]:'none',
		class : (type === TYPE.FOLDER ? "folder " : "file ") + "level_" + folders.length + " " + folders.join(' ')
	}}
function buildTreeNode(id){
	let level = id === ""?0:id.split('/').length;
	
	let children = pages.filter(
		page => page.folder.contains(id)
	);
	
	[...new Set(
		children.filter(
			page => page.folder != id
		).map(
			page => page.folder.split('/').slice(0,level+1).join("/")
		)
	)].map(
		folder => {
			let folder_info = getInfo(folder,TYPE.FOLDER);
			dv.el(
				"div"
				, folder
				, { 
					cls: folder_info.class,
					attr : {
						id : folder_info.id,
						parent_id : folder_info.parent_id
					}
				}
			)
			buildTreeNode(folder);
			dv.el(
				"div"
				, ""
				, { 
					cls: "folder_separator",
				}
			);
		}
	)
	children.filter(
		page => page.folder == id
	).map(
		page => {		
			let file_info = getInfo(page.folder,TYPE.FILE);
			dv.el(
				"div"
				, page.link
				, { 
					cls: file_info.class,
					attr : {
						parent_id : file_info.parent_id
					}
				}
			);
		}
	);
}
buildTreeNode("");
```
