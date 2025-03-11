<template>
    <div class="select-container">
        <div class="column">
            <h3>Available Options</h3>
            <div
              v-for="option in availableOptions"
              :key="option.id"
              class="option available"
              @click="toggleOption(option)"
            >
                {{ option.label }}    
            </div>

        </div>

        <div class="column">
            <h3>Disable Options</h3>
            <div
              v-for="option in disabledOptions"
              :key="option.id"
              class="option disabled"
              @click="toggleOption(option)"
            >
                {{ option.label }}    
            </div>

        </div>
    </div>
</template>

<script setup>
    import {defineProps, ref, computed, defineEmits} from 'vue'

    const props = defineProps({
      field: Object, 
      modelValue: Array, 
    });

    const emit = defineEmits(["update"]);

    const localOptions = ref(
    props.field.options.map((opt) => ({
      ...opt,
      disabled: props.modelValue?.includes(opt.id) || false, 
    }))
  );

    const availableOptions = computed(() =>localOptions.value.filter((item) => !item.disabled))
    const disabledOptions = computed(() => localOptions.value.filter((item) => item.disabled))

    const toggleOption = (option) => {
      console.log(option)
      option.disabled = !option.disabled;

      emit("update", {id: props.field.id, value: disabledOptions.value.map((item) => item.id)})
    }

</script>



<style scoped>
.select-container {
  display: flex;
  justify-content: space-between;
  width: 100%;
  gap: 20px;
}

.column {
  width: 45%;
  border: 1px solid #ccc;
  padding: 10px;
  background: #222;
}

h3 {
  text-align: center;
  color: #fff;
  margin-bottom: 10px;
}

.option {
  padding: 8px;
  margin: 5px 0;
  cursor: pointer;
  border-radius: 5px;
  text-align: center;
}

.option.available {
  background: #444;
  color: #fff;
}

.option.disabled {
  background: #777;
  color: #bbb;
}
</style>
