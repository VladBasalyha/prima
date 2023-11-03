<script setup>
	const props = defineProps(["product"]);

	const { product } = toRefs(props);
</script>
<template>
	<div class="flex justify-start my-2">
		<img width="100" height="100" class="rounded-md md:w-[100px] h-[120px]" :src="product.url" alt="product image" />

		<div class="overflow-hidden pl-2">
			<div class="flex items-center">
				<span class="bg-red-400 text-white text-[9px] font-semibold"> </span>
				<div class="truncate pl-2 font-semibold text-md">
					{{ product.title }}
				</div>
			</div>

			<div class="text-lg font-light">
				$ <span class="font-light">{{ product.price / 100 }}</span>
			</div>
		</div>
	</div>
</template>
