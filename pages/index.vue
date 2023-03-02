<template>
	<div>
		<Files :file="fileTree"/>
	</div>
</template>

<script setup>
import {getData} from "~/services/files";
import Files from "../components/Files";

const fileNames = getData()

let fileTree = [];
let level = {fileTree};

fileNames.forEach(path => {
	path.split('/').reduce((result, pathName) => {
		if(!result[pathName]) {
			result[pathName] = {fileTree: []};
			result.fileTree.push({pathName, children: result[pathName].fileTree})
		}

		return result[pathName]
	}, level)
})

console.log(fileTree)

</script>