<template>
	<div>
		<client-only>
			<input
				class="w-full bg-gray-300 text-gray-700 border text-sm font-bold border-gray-600 rounded-lg p-3 placeholder-gray-500 focus:outline-none"
				:placeholder="placeholder"
				:maxlength="max"
				@focus="isFocused = true"
				@blur="isFocused = false"
				:type="inputType"
				v-model="inputComputed"
				autocomplete="on"
				:class="({ 'border-purple-950': isFocused }, { 'border-red-300': error })"
			/>
		</client-only>

		<span v-if="error" class="text-red-300 text-[14px] font-semibold">
			{{ error }}
		</span>
	</div>
</template>

<script setup>
	const emit = defineEmits(["update:input"]);
	const props = defineProps(["input", "placeholder", "max", "inputType", "error"]);

	const { input, placeholder, max, inputType, error } = toRefs(props);

	let isFocused = ref(false);

	const inputComputed = computed({
		get: () => input.value,
		set: (val) => emit("update:input", val),
	});
</script>
