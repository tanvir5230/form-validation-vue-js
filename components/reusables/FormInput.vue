<template>
  <div>
    <label :for="id" class="block text-sm font-medium text-gray-700">{{ label }}</label>
    <input
      :value="modelValue"
      :type="type"
      :id="id"
      @input="onInput"
      @blur="validate"
      class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
    />
    <span v-if="error" class="text-sm text-red-600 mt-2 block">{{ error }}</span>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: 'text',
    },
    id: {
      type: String,
      required: true,
    },
    modelValue: {
      type: String,
      required: true,
    },
    error: {
      type: String,
      default: '',
    },
    validate: {
      type: Function,
      required: true,
    },
  },
  emits: ['update:modelValue'],
  methods: {
    onInput(event) {
      // Emit the event to update the parent component's value
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>
