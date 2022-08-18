<template>
  <MultiStep :schema="schema" :step="step" @submit="formSubmit">
    <template #okay>
      <BaseButton v-if="step > 0" @click="step--">Back</BaseButton>
      <BaseButton v-if="step < 1" type="submit">Next</BaseButton>
      <BaseButton>Submit</BaseButton>
    </template>
  </MultiStep>

  <pre>{{ userData }}</pre>
</template>

<script>
import { ref, computed } from "vue";
import FormText from "@/components/FormText.vue";
import FormSelect from "@/components/FormSelect.vue";
import MultiStep from "@/components/MultiStep.vue";
import BaseButton from "@/components/BaseButton.vue";
import { useSchemaForm } from "formvuelate";
import * as yup from "yup";

const SCHEMA = [
  {
    firstName: {
      component: FormText,
      label: "First Name",
      validations: yup.string().required("Firstname is required"),
    },
    lastName: {
      component: FormText,
      label: "Last Name",
      validations: yup.string().required(),
    },
  },
  {
    email: {
      component: FormText,
      label: "Your email",
      validations: yup.string().required(),
      config: {
        type: "email",
      },
    },
  },
];

export default {
  components: { BaseButton, MultiStep },
  setup() {
    const userData = ref({});
    useSchemaForm(userData);

    const schema = computed(() => {
      if (!userData.value.email) return SCHEMA;

      return [
        { ...SCHEMA[0] },
        {
          ...SCHEMA[1],
          favoriteThingAboutVue: {
            component: FormSelect,
            label: "Favorite thing about Vue",
            required: true,
            options: ["Ease of use", "Documentation", "Community"],
          },
        },
      ];
    });

    const step = ref(0);

    const formSubmit = () => {
      console.log(userData.value);
      step.value++;
    };

    return {
      userData,
      schema,
      formSubmit,
      step,
    };
  },
};
</script>


<style lang="scss">
@import "./assets/styles.scss";
</style>