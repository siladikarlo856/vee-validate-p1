<template>
  <input v-model="value" placeholder="Type something" />
  <span>{{ errorMessage }}</span>
</template>

<script>
import { useField } from 'vee-validate';
import * as yup from 'yup';

export default {
  name: 'EditorComponent',
  setup() {
    const { value, errorMessage } = useField(
      'fullName',
      yup
        .string()
        .required('Ovo polje je obavezno')
        .min(8, 'minimalni broj znakova je 8')
    );

    return {
      value,
      errorMessage,
    };
  },
};
</script>
