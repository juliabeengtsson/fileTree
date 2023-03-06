<template>
	<div class="gap-x-10 mx-20 mt-20">
		<h2 class="mb-10 underline font-semibold text-2xl">Mina filer</h2>

		<div class="grid grid-cols-3 gap-3">
			<div class="col-span-1 bg-gray-200 py-5 pl-5 rounded">
				<!-- Loop for first step in fileTree -->
				<div v-for="file of createFileTree()" class="py-3">
					<!-- fileTree with folders -->
					<div v-if="Object.keys(file.files).length" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FolderIcon />
							<p>{{ file.pathName }}</p>
							<ArrowIcon />
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
					<!-- fileTree without folder - (files) -->
					<div v-if="!Object.keys(file.files).length" class="cursor-pointer flex items-center justify-between relative">
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
				<div class="cursor-pointer font-semibold flex items-center gap-3" @click="closeAll()" v-if="Object.keys(openedFiles).length > 0">
					<LeftArrowIcon />
					<p>Stäng</p>
				</div>

				<!-- Loop for folders with under files -->
				<div v-for="file of openedFiles" class="py-3">
					<!-- second folder with childrens -->
					<div v-if="Object.keys(file.files).length" class="cursor-pointer flex items-center justify-between relative">
						<div class="flex gap-3 items-center" @click="showFiles(file)">
							<FolderIcon />
							<p>{{ file.pathName }}</p>
							<ArrowIcon />
						</div>
						<div class="mr-5">
							<ContextMenu />
						</div>
					</div>
					<!-- second folder without under files -->
					<div v-if="!Object.keys(file.files).length" class="cursor-pointer flex items-center justify-between relative">
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
const openedFiles = ref([]) //

// Function for open folders and files
function showFiles(_files) {
	openedFiles.value = _files.files
}

function createFileTree() {
	const fileTree = {}

	// Loop every file path
	fileNames.forEach(filePath => {
		let currentFileTree = fileTree

		// // Create a list of every path of the file path and loop through it
		filePath.split('/').forEach(partOfPath => {
			// If the file tree does not contain an object with the same name as the current part of the path
			if(!currentFileTree[partOfPath]) {
				// Add the file path to the file tree and set the attributes
				currentFileTree[partOfPath] = { pathName: partOfPath, files: {}}
			}
			// Change current path to the files-attribute
			currentFileTree =  currentFileTree[partOfPath].files
		})
	})
	return fileTree
}

// Function for close all folders and files
function closeAll() {
	openedFiles.value = []
}
</script>