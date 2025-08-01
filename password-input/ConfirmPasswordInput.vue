<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { EyeIcon, EyeOffIcon } from 'lucide-vue-next'

import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'

// Define props
const props = defineProps<{
  id?: string
  label?: string
  placeholder?: string
  modelValue?: string
  class?: string
  required?: boolean
  tabindex?: number
  autocomplete?: string
}>()

// Define emits
const emits = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

// Generate a unique ID if not provided
const uniqueId = ref('')
onMounted(() => {
  uniqueId.value = props.id || `confirm-password-input-${Math.random().toString(36).substring(2, 9)}`
})

// Create a ref for the password and bind it to modelValue
const password = computed({
  get: () => props.modelValue || '',
  set: (value) => emits('update:modelValue', value)
})

// Password visibility state
const isVisible = ref(false)

// Toggle password visibility
const toggleVisibility = () => {
  isVisible.value = !isVisible.value
}

// Default export
defineExpose({
  password,
  isVisible,
  toggleVisibility
})
</script>

<template>
  <div :class="props.class">
    <!-- Password input field with toggle visibility button -->
    <div class="*:not-first:mt-2">
      <Label v-if="label" :for="uniqueId">{{ label }}</Label>
      <div class="relative">
        <Input
          :id="uniqueId"
          class="pe-9"
          :placeholder="placeholder || 'Confirm Password'"
          :type="isVisible ? 'text' : 'password'"
          v-model="password"
          :required="required"
          :tabindex="tabindex"
          :autocomplete="autocomplete"
        />
        <button
          class="text-muted-foreground/80 hover:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 absolute inset-y-0 end-0 flex h-full w-9 items-center justify-center rounded-e-md transition-[color,box-shadow] outline-none focus:z-10 focus-visible:ring-[3px] disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50"
          type="button"
          @click="toggleVisibility"
          :aria-label="isVisible ? 'Hide password' : 'Show password'"
          :aria-pressed="isVisible"
          aria-controls="password"
        >
          <EyeOffIcon v-if="isVisible" :size="16" aria-hidden="true" />
          <EyeIcon v-else :size="16" aria-hidden="true" />
        </button>
      </div>
    </div>
  </div>
</template>
