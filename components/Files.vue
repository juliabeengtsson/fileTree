<template>
	<div class="">
		<div v-for="loop of file" class="">
			<div v-if="loop.children.length > 0" class="bg-red-400 ml-10">
				<Folder :file="loop" />
			</div>

			<div v-else class="bg-blue-900">
				<p>{{loop.pathName}}</p>
			</div>
		</div>
	</div>
</template>

<script setup>

import Folder from "./Folder";

const props = defineProps({
	file: {}
})

const isHidden = ref(true)
</script>