<template>
  <div>
    <label for="">Required string</label>
    <input
      v-model="requiredString"
      placeholder="Type string"
      @input="onNameInput"
    />
    <span>{{ requiredErrorMessage }}</span>
  </div>
  <div>
    <label for="">Optional string</label>
    <input v-model="optionalString" placeholder="Type number" />
    <span>{{ optionalErrorMessage }}</span>
  </div>
  <button @click="onSendClick">Send</button>

  <div>
    <p>Props: { name:{{ name }}, description:{{ description }} }</p>

    <p>
      toRef: { fullNameRef:{{ fullNameRef }}, descriptionRef:{{
        descriptionRef
      }}
      }
    </p>
  </div>

  <button @click="onHandleChangeClick">handleChange</button>
</template>

<script>
import { toRef, watch } from 'vue';
import { useField } from 'vee-validate';
import * as yup from 'yup';

export default {
  name: 'EditorComponent',
  props: {
    name: String,
    description: { type: String },
  },
  setup(props, context) {
    const fullNameRef = toRef(props, 'name');
    const descriptionRef = toRef(props, 'description');

    const {
      name: requireName,
      value: requiredString,
      errorMessage: requiredErrorMessage,
      validate: validateRequiredString,
      handleChange,
    } = useField(
      fullNameRef,
      yup
        .string()
        .required('Ovo polje je obavezno')
        .min(8, 'minimalni broj znakova je 8'),
      {
        initialValue: props.name,
        validateOnMount: true,
      }
    );

    const {
      value: optionalString,
      errorMessage: optionalErrorMessage,
      validate: validateOptionalString,
    } = useField(
      descriptionRef,
      yup.string().min(3, 'minimalni broj znakova je 3'),
      {
        initialValue: descriptionRef.value,
      }
    );

    function onSendClick() {
      Promise.all([validateRequiredString(), validateOptionalString()])
        .then((value) => {
          console.log('validation', value);
          if (value[0].valid && value[1].valid) {
            console.log('model', requiredString.value, optionalString.value);
          } else {
            throw new Error(value.errors);
          }
        })
        .catch((err) => {
          console.error('Error', err);
        });
    }

    function onHandleChangeClick() {
      console.log('click');
      handleChange('a');
    }

    function onNameInput(e) {
      console.log('onNameInput', e, e.target.value);
      context.emit('update:name', e.target.value);
    }

    watch(
      () => props.name,
      (newValue, oldValue) => {
        console.log('watch(() => props.name', newValue, oldValue);
        handleChange(newValue);
      }
    );

    // LIFE-CYCLE
    // On create
    console.log('name on create:', requireName, props.name);
    console.table(requireName)
    // requiredString.value = props.name;

    return {
      requiredString,
      requiredErrorMessage,
      optionalString,
      optionalErrorMessage,
      onSendClick,

      fullNameRef,
      descriptionRef,

      onHandleChangeClick,
      onNameInput,
    };
  },
};
</script>
