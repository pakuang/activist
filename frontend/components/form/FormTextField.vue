<template>
  <div
    class="flex items-center pl-[12px] pr-[10px] py-2 max-h-[40px] space-x-2 text-left border rounded select-none text-light-distinct-text dark:text-dark-distinct-text"
    :class="{
      'border-light-action-red dark:border-dark-action-red': error,
      'border-light-interactive dark:border-dark-interactive': !error,
    }"
  >
    <input
      @input="updateValue"
      @blur="emit('blurred')"
      @focus="emit('focused')"
      :id="uuid"
      class="w-full h-5 bg-transparent outline-none placeholder:text-light-distinct-text dark:placeholder:text-dark-distinct-text"
      :value="modelValue"
      :placeholder="placeholder"
      :type="refInputType"
    />
    <span
      v-for="(i, index) in icons"
      @click="handleIconClick(i)"
      :key="index"
      class="cursor-pointer"
    >
      <Icon
        v-if="i === 'bi:eye-fill' && refInputType === 'password'"
        :name="i"
        size="1.4em"
      />
      <Icon
        v-else-if="i === 'bi:eye-fill' && refInputType === 'text'"
        name="bi:eye-slash-fill"
        size="1.4em"
      />
      <Icon v-else :name="i" size="1.2em" :color="getIconColor(i)" />
    </span>
  </div>
</template>

<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import useFormInput from "~/composables/useFormSetup";

export interface Props {
  placeholder?: string;
  modelValue?: string;
  inputType?: string;
  isIconVisible?: boolean;
  icons?: string[];
  error?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: "",
  modelValue: "",
  inputType: "text",
  isIconVisible: false,
  error: false,
});

const emit = defineEmits(["update:modelValue", "blurred", "focused"]);
const { updateValue } = useFormInput({ value: props?.modelValue }, emit, false);
const uuid = uuidv4();
const refInputType = ref<string | undefined>(props?.inputType);
const changeInputType = () => {
  refInputType.value = refInputType.value === "password" ? "text" : "password";
};

const handleIconClick = (icon: string) => {
  if (icon === "bi:eye-fill") {
    changeInputType();
  }
};

const getIconColor = (icon: string) => {
  if (icon === "bi:check-lg") {
    return "#3BA55C";
  } else if (icon === "bi:x-lg") {
    return "#BA3D3B";
  } else {
    return "#5A5A5A";
  }
};
</script>
