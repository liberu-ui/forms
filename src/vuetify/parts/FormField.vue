<template>
    <div class="field"
        :class="$attrs.class">
        <component v-bind="$attrs"
            :is="fieldType(field)"
            :field="field"
            :errors="errors"
            :http="http"
            :i18n="i18n"
            :locale="locale"
            @changed="autosave"
            ref="field"/>
        <slot/>
    </div>
</template>

<script>
import 'v-tooltip/dist/v-tooltip.css';
import { VTooltip } from 'v-tooltip';
import { debounce } from 'lodash';
import { FontAwesomeIcon as Fa } from '@fortawesome/vue-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faInfoCircle } from '@fortawesome/free-solid-svg-icons';


import SwitchField from '../fields/SwitchField.vue';
import InputField from '../fields/InputField.vue';
import MoneyField from '../fields/MoneyField.vue';
import DateField from '../fields/DateField.vue';
import TimeField from '../fields/TimeField.vue';
import SelectField from '../fields/SelectField.vue';
import TextareaField from '../fields/TextareaField.vue';
import WysiwygField from '../fields/WysiwygField.vue';

library.add(faInfoCircle);

export default {
    name: 'FormField',

    directives: { tooltip: VTooltip },

    components: {
        Fa,
        DateField,
        InputField,
        MoneyField,
        SwitchField,
        SelectField,
        TextareaField,
        TimeField,
        WysiwygField,
      //Vuetify components
    },

    inject: ['fieldType', 'errors', 'http', 'i18n', 'locale', 'state', 'submit'],

    inheritAttrs: false,

    props: {
        field: {
            type: Object,
            required: true,
        },
    },

    created() {
        this.autosave = debounce(this.autosave, this.state.data.debounce);

        if (!this.state.data.labels && !this.field.meta.placeholder) {
            this.field.meta.placeholder = this.field.label;
        }
    },

    methods: {
        autosave() {
            if (this.state.data.autosave) {
                this.errors.empty();
                this.submit();
            }
        },
    },
};
</script>
