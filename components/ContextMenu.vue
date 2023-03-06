<template>
	<div>
		<div class="mr-5 p-3" @click="toggle()">
			<ContextIcon />
		</div>
		<!-- Dropdown menu for delete and editing -->
		<div v-if="isHidden" id="dropdown" class="absolute right-1 top-8 z-10 w-44 bg-white rounded divide-y divide-gray-100 shadow block">
			<ul class="py-1 text-sm text-gray-700"
				aria-labelledby="dropdownMenuIconHorizontalButton">
				<li>
					<a href="#" class="block py-2 px-4 hover:bg-gray-100">Redigera</a>
				</li>
				<li>
					<a href="#" class="block py-2 px-4 hover:bg-gray-100">Ta bort</a>
				</li>
			</ul>
		</div>
	</div>
</template>

<script setup>
import ContextIcon from "./svgs/ContextIcon";

const isHidden = ref()

function toggle() {
	isHidden.value = !isHidden.value
}

</script>