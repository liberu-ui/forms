<template>
    <wysiwyg v-bind="field.meta"
        v-model="field.value"
        :api-key="field.meta.apiKey"
        :plugins="field.meta.plugins"
        :toolbar="field.meta.toolbar"
        :has-error="errors.has(field.name)"
        @update:model-value="errors.clear(field.name); $emit('changed')"/>
</template>

<script>
import Wysiwyg from '@liberu-ui/wysiwyg/bulma';

export default {
    name: 'WysiwygField',

    components: { Wysiwyg },

    inject: ['errors'],

    props: {
        field: {
            type: Object,
            required: true,
        },
    },

    emits: ['changed'],
};
</script>
