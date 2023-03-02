<template>
	<div>
		<p @click="toggle()">{{ props.file.pathName }} :::::</p>

		<div :class="{'hidden' : isHidden}">
			<Files :file="props.file.children" />
		</div>
	</div>
</template>

<script setup>
import Files from "./Files";

const props = defineProps({
	file: {}
})

const isHidden = ref(true)

function toggle() {
	isHidden.value = !isHidden.value
}

</script>