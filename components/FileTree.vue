<template>
	<div class="gap-x-10 mx-20 mt-20">
		<h2 class="mb-10 underline font-semibold text-2xl">Mina filer</h2>

		<div class="grid grid-cols-3 gap-3">
			<div class="col-span-1 bg-gray-200 py-5 pl-5 rounded">
				<!-- Loop for first step in fileTree -->
				<div v-for="file of fileTree" class="py-3">
					<!-- fileTree with childrens (folders) -->
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
					<!-- fileTree without childrens (files) -->
					<div v-if="file.children.length <= 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FileIcon />
							<p>{{ file.pathName }}</p>
						</div>
						<!-- Dropdown menu -->
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
				</div>
			</div>

			<div class="col-span-2 bg-gray-200 py-5 pl-5 rounded">
				<!-- Close all folders button -->
				<div class="cursor-pointer font-semibold flex items-center gap-3" @click="openPrevious()" v-if="openedFiles.length > 0">
					<LeftArrowIcon />
					<p>Stäng</p>
				</div>
				<!-- Loop for folders with childrens -->
				<div v-for="file of openedFiles" class="py-3">
					<!-- second folder with childrens -->
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
					<!-- second folder without childrens (files) -->
					<div v-if="file.children.length <= 0" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FileIcon />
							<p>{{ file.pathName }}</p>
						</div>
						<!-- Dropdown menu -->
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

const fileNames = getData() // get data from services
const openedFiles = ref([])

let fileTree = []; // created an empty array to use in forEach function and push the result
let level = {fileTree};

fileNames.forEach(path => {
	// use .split to creat an array for each object
	path.split('/').reduce((result, pathName) => {
		if(!result[pathName]) {
			result[pathName] = {fileTree: []};
			result.fileTree.push({pathName, children: result[pathName].fileTree})
		}
		return result[pathName]
	}, level)
})

// Function for open folders and files
function showFiles(filesToOpen) {
	if(filesToOpen.children.length > 0) {
		openedFiles.value = filesToOpen.children
	}
}

// Function for close all folders and files
function openPrevious() {
	openedFiles.value = []
}
</script>