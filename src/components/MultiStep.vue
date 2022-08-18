<template>
  <form @submit.prevent="$emit('submit', $event)">
    <SchemaFormWithValidation :schema="currentSchema">
      <template #afterForm>
        <slot name="okay" />
      </template>
    </SchemaFormWithValidation>
  </form>
</template>

<script>
import { computed, provide } from "vue";
import VeeValidatePlugin from "@formvuelate/plugin-vee-validate";
import { SchemaFormFactory } from "formvuelate";
const SchemaFormWithValidation = SchemaFormFactory([
  VeeValidatePlugin({
    // plugin configuration here
  }),
]);

export default {
  name: "MultiStep",
  components: { SchemaFormWithValidation },
  props: {
    schema: {
      type: Array,
      required: true,
    },
    step: {
      type: Number,
      required: true,
      default: 0,
    },
  },
  emits: ["submit"],
  setup(props) {
    // provide(`fvl_isSchemaWizard`, true);

    const currentSchema = computed(() => {
      return props.schema[props.step];
    });

    return { currentSchema };
  },
};
</script>
