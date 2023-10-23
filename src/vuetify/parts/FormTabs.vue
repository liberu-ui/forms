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


  <div>
    <v-tabs
        v-model="tabRef"
        bg-color="primary"
        align-tabs="center"
    >
      <v-tab keep-alive
             :id="section.tab"
             :key="index + '_' + section.tab"
             v-bind="$attrs"
             v-for="(section, index) in sections()" :value="section.tab"
      >
        {{ i18n(section.tab) }}
        <span class="tag is-danger is-small error-count"
              v-if="errorCount(section.tab)">
                {{ errorCount(section.tab) }}
            </span>
      </v-tab>
    </v-tabs>
    <v-window v-model="tabRef">
      <v-window-item
          v-for="(section, index) in sections()"
          :key="index + '-tab-sections'"
          :value="section.tab"
      >
        <form-section :key="index + '-form-section'"
                      :section="section">
          <template v-for="field in sectionCustomFields(section)"
                    #[field.name]>
            <slot :name="field.name"
                  v-bind="fieldBindings(field)"/>
          </template>
        </form-section>
      </v-window-item>
    </v-window>
  </div>
</template>

<script setup>
import { EnsoTabs as Tabs, Tab } from '@liberu-ui/tabs';
import {inject, ref} from "vue";
import FormSection from './FormSection.vue';

const tabRef = ref(null)

const errorCount = inject('errorCount', /* default value if not provided */);
const fieldBindings = inject('fieldBindings', /* default value if not provided */);
const sectionCustomFields = inject('sectionCustomFields', /* default value if not provided */);
const i18n = inject('i18n', /* default value if not provided */);
const sections = inject('sections', /* default value if not provided */);
const tabs = inject('tabs', /* default value if not provided */);
const visibleSection = inject('visibleSection', /* default value if not provided */);

export default {
    name: 'FormTabs',

    components: { Tabs, Tab, FormSection },

};
</script>
