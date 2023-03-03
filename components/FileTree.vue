<template>
	<div class="gap-x-10 mx-20 mt-20">
		<h2 class="mb-10 underline font-semibold text-2xl">Mina filer</h2>

		<div class="grid grid-cols-3 gap-3">
			<div class="col-span-1 bg-gray-200 py-5 pl-5 rounded">
				<div v-for="file of fileTree" class="py-3">
					<div v-if="file.children.length > 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FolderIcon />
							<p>{{ file.pathName }}</p>
							<ArrowIcon />
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
					<div v-if="file.children.length <= 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FileIcon />
							<p>{{ file.pathName }}</p>
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
				</div>
			</div>

			<div class="col-span-2 bg-gray-200 py-5 pl-5 rounded">
				<div class="cursor-pointer font-semibold flex items-center gap-3" @click="openPrevious()" v-if="openedFiles.length > 0">
					<LeftArrowIcon />
					<p>Stäng</p>
				</div>
				<div v-for="file of openedFiles" class="py-3">
					<div v-if="file.children.length > 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FolderIcon />
							<p>{{ file.pathName }}</p>
							<ArrowIcon />
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
					<div v-if="file.children.length <= 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FileIcon />
							<p>{{ file.pathName }}</p>
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup>
import {getData} from "~/services/files";
import FolderIcon from "./svgs/FolderIcon";
import FileIcon from "./svgs/FileIcon";
import ArrowIcon from "./svgs/ArrowIcon";
import LeftArrowIcon from "./svgs/LeftArrowIcon";
import ContextMenu from "./ContextMenu";

const fileNames = getData()
const openedFiles = ref([])

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

function showFiles(filesToOpen) {
	if(filesToOpen.children.length > 0) {
		openedFiles.value = filesToOpen.children
	}
}

function openPrevious() {
	openedFiles.value = []
}
</script>