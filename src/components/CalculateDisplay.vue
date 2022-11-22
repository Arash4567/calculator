<template>
  <div class="flex flex-col items-end justify-end w-full px-1 pt-1 mt-2 text-right text-gray-600 bg-white rounded-md h-2/5">
    <div class="w-full overflow-y-auto" id="resultElement"	>
      <div class="space-y-3" >
				<div class="-space-y-1" v-for="history in histroryOperation" :key="history.operation">
					<p class="text-lg font-light">{{ history.operation }}</p>
					<p class="text-2xl font-medium">= {{ history.result }}</p>
				</div>
			</div>
    </div>
    <div class="z-10 flex flex-col items-end w-full">
      <p class="text-2xl">{{ lastOperation != '0' ? lastOperation : '' }}</p>
      <div id="numberInput" ref="numberInput" class="z-0 w-full overflow-x-auto overflow-y-hidden scroll">
        <p class="text-5xl font-semibold">
          {{ lastOperation != '0' ? '=' : '' }}{{ currentCalculate }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'CalculateHistory',
    props: {
      histroryOperation: Array,
      lastOperation: String,
      currentCalculate: String,
			deleteHistory: Function
    }
  }
</script>

<style scoped>

</style>
