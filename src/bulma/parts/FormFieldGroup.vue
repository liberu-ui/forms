<template>
    <div class="form-field"
        :class="$attrs.class">
        <div class="field">
            <label class="label"
                v-if="state.data.labels">
                {{ i18n(field.label) }}
                <span class="icon is-small has-text-info"
                    v-tooltip="i18n(field.meta.tooltip)"
                    v-if="field.meta.tooltip">
                    <fa icon="info-circle"
                        size="xs"/>
                </span>
            </label>
        </div>
        <div :class="['field', { 'has-addons': hasAddons }, { 'is-grouped': isGrouped }]">
            <slot name="left"/>
            <div class="control is-expanded">
                <component :class="{ 'is-fullwidth': hasAddons }"
                    v-bind="$attrs"
                    :is="fieldType(field)"
                    :field="field"
                    :errors="errors"
                    :http="http"
                    :i18n="i18n"
                    :locale="locale"
                    @changed="autosave"
                    ref="field"/>
            </div>
            <slot name="right"/>
            <p class="help is-danger"
                v-if="errors.has(field.name)">
                {{ errors.get(field.name) }}
            </p>
        </div>
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
        InputField,
        DateField,
        MoneyField,
        SelectField,
        SwitchField,
        TextareaField,
        TimeField,
        WysiwygField,
    },

    inject: [
        'fieldType', 'errors', 'http', 'i18n', 'locale', 'state', 'submit',
    ],

    inheritAttrs: false,

    props: {
        field: {
            type: Object,
            required: true,
        },
        hasAddons: {
            type: Boolean,
            default: false,
        },
        isGrouped: {
            type: Boolean,
            default: false,
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
                this.submit();
            }
        },
    },
};
</script>
