<template>
    <tabs class="form-tabs">
        <template #label="{ tab }">
            {{ i18n(tab) }}
            <span class="tag is-danger is-small error-count"
                v-if="errorCount(tab)">
                {{ errorCount(tab) }}
            </span>
        </template>
        <tab keep-alive
            :id="tab"
            :key="tab"
            v-bind="$attrs"
            v-for="tab in tabs()">
            <template v-for="(section, index) in sections(tab)">
                <slot :name="section.slot"
                    v-if="section.columns === 'slot'"/>
                <form-section :key="index"
                    :section="section"
                    v-else-if="visibleSection(section)">
                    <template v-for="field in sectionCustomFields(section)"
                        #[field.name]>
                        <slot :name="field.name"
                            v-bind="fieldBindings(field)"/>
                    </template>
                </form-section>
            </template>
        </tab>
    </tabs>
</template>

<script>
import { EnsoTabs as Tabs, Tab } from '@liberu-ui/tabs/bulma';
import FormSection from './FormSection.vue';

export default {
    name: 'FormTabs',

    components: { Tabs, Tab, FormSection },

    inject: [
        'errorCount', 'fieldBindings', 'sectionCustomFields',
        'i18n', 'sections', 'tabs', 'visibleSection',
    ],
};
</script>
