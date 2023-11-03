<template>
	<MainLayout>
		<div id="SuccessPage" class="mt-20 max-w-[900px] mx-auto min-h-[50vh]">
			<div class="flex items-center bg-gray-200 w-full p-2 min-h-[120px]">
				<div class="flex items-center text-xl m-auto">
					<Icon name="icon-park-twotone:success" color="5fcb03" size="35" />
					<span class="pl-4 font-bold text-center">Payment successful</span>
					<p class="text-sm font-extrabold pl-[50px] text-center">Thank you for choosing Prima! Payment was received</p>
				</div>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';

	import { useUserStore } from '~/stores/user';

	const userStore = useUserStore();

	onMounted(() => {
		setTimeout(() => {
			userStore.isLoading = false;
		}, 200);
	});
</script>
