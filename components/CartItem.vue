<script setup>
	import { useUserStore } from '~/stores/user';
	const props = defineProps(['product', 'selectedArray']);

	const { product, selectedArray } = toRefs(props);

	const emit = defineEmits(['selectedRadio']);
	let isHover = ref(false);
	let isSelected = ref(false);
	const userStore = useUserStore();

	const priceComputed = computed(() => {
		return product.value.price / 10;
	});

	const oldPriceComputed = computed(() => {
		let res = (product.value.price + product.value.price / 20) / 100;
		return res.toFixed(2);
	});

	const removeFromCart = () => {
		userStore.cart.forEach((prod, index) => {
			if (prod.id === product.value.id) {
				userStore.cart.splice(index, 1);
			}
		});
	};

	watch(
		() => isSelected.value,
		(val) => {
			emit('selectedRadio', { id: product.value.id, val: val });
		},
	);
</script>

<template>
	<div class="flex justify-start my-2 items-center">
		<div class="my-auto">
			<div class="flex items-center justify-start p-1 cursor-pointer" @mouseenter="isHover = true" @mouseleave="isHover = false">
				<div
					class="flex items-center justify-center border mr-5 ml-2 h-[20px] w-[20px] rounded-full"
					:class="[isHover ? 'border-gray-100' : 'border-gray-400', isSelected ? 'bg-gray-600' : 'bg-gray-400']"
					@click="isSelected = !isSelected">
					<div class="h-[8px] rounded-full bg-gray-200"></div>
				</div>
			</div>
		</div>

		<img width="100" height="100" class="rounded-md md:w-[100px] h-[120px]" :src="product.url" alt="product image" />
		<div class="overflow-hidden pl-2 w-full">
			<div class="flex items-center justify-between w-full">
				<div class="flex items-center justify-between truncate">
					<span class="sm:block hidden bg-[#fd2871] text-white text-[10px] font-semibold px-1.5 rounded-sm min-w-[80px]">Welcome Deal</span>
					<div class="truncate sm:pl-2">{{ product.title }}</div>
				</div>

				<button class="block items-center -mt-0.5 hover:text-red-500" @click="removeFromCart()">
					<Icon name="ic:baseline-delete-forever" size="20" />
				</button>
			</div>

			<div class="text-xl font-semibold">
				$
				<span class="font-bold">
					{{ product.price / 100 }}
				</span>
			</div>
			<p class="text-xs font-semibold pt-1 text-[#008a55]">free delivery over 10$</p>
		</div>
	</div>
</template>
