<template>
    <vue-form
        :http="http"
        :i18n="i18n"
        :locale="lang"
        :path="path"
        :error-handler="errorHandler"
        :router-error-handler="routerErrorHandler"
        :disable-state="disableState"
        @submit="success"
        @destroy="success"
        @ready="init"
        ref="form">
        <template v-for="customField in customFields"
            #[customField.name]="props">
            <slot :name="customField.name"
                v-bind="props"/>
        </template>
        <template v-for="section in customSections"
            #[section.slot]="props">
            <slot :name="section.slot"
                v-bind="props"/>
        </template>
        <template v-for="actions in ['actions-right', 'actions-left']"
            #[actions]>
            <slot :name="actions"/>
        </template>
    </vue-form>
</template>

<script>
import { mapGetters, mapMutations } from 'vuex';
import VueForm from './VueForm.vue';

export default {
    name: 'EnsoForm',

    components: { VueForm },

    inject: ['errorHandler', 'http', 'i18n', 'route', 'routerErrorHandler', 'toastr'],

    props: {
        disableState: {
            type: Boolean,
            default: false,
        },
    },

    data: () => ({
        ready: false,
    }),

    computed: {
        ...mapGetters('preferences', ['lang', 'bookmarks']),
        ...mapGetters('bookmarks', ['state']),
        customFields() {
            return this.ready
                ? this.$refs.form.customFields
                : [];
        },
        customSections() {
            return this.ready
                ? this.$refs.form.customSections
                : [];
        },
        data() {
            return this.ready
                ? this.$refs.form.data
                : [];
        },
        formData() {
            return this.ready
                ? this.$refs.form.formData
                : [];
        },
        dirty() {
            return this.ready
                ? this.$refs.form.dirty
                : false;
        },
        dirtyFields() {
            return this.ready
                ? this.$refs.form.dirtyFields
                : false;
        },
        errors() {
            return this.ready
                ? this.$refs.form.errors
                : null;
        },
        path() {
            return this.$attrs.path
                ?? this.route(this.$route.name, this.$route.params, false);
        },
    },

    watch: {
        formData: {
            handler(formData) {
                if (this.disableState || !this.bookmarks) {
                    return;
                }

                if (!this.dirty) {
                    this.updateState({ bookmark: this.$route });
                    return;
                }

                this.updateState({
                    bookmark: this.$route,
                    data: JSON.parse(JSON.stringify(formData)),
                });
            },
            deep: true,
        },
        dirty(dirty) {
            if (!this.disableState && !dirty && this.bookmarks) {
                this.updateState({ bookmark: this.$route });
            }
        },
    },

    methods: {
        ...mapMutations('bookmarks', ['updateState']),
        init() {
            this.ready = true;

            if (this.disableState || !this.bookmarks) {
                return;
            }

            const state = this.state(this.$route);

            if (state) {
                this.fill(state);
            }
        },
        fetch() {
            return this.ready
                && this.$refs.form.fetch();
        },
        submit() {
            return this.ready
                && this.$refs.form.submit();
        },
        field(field) {
            return this.ready
                && this.$refs.form.field(field);
        },
        param(param) {
            return this.ready
                && this.$refs.form.param(param);
        },
        routeParam(param) {
            return this.ready
                && this.$refs.form.routeParam(param);
        },
        fill(state) {
            return this.ready
                && this.$refs.form.fill(state);
        },
        setOriginal() {
            return this.ready
                && this.$refs.form.setOriginal();
        },
        undo() {
            return this.ready
                && this.$refs.form.undo();
        },
        hideTab(tab) {
            return this.ready
                && this.$refs.form.hideTab(tab);
        },
        showTab(tab) {
            return this.ready
                && this.$refs.form.showTab(tab);
        },
        hideField(field) {
            return this.ready
                && this.$refs.form.hideField(field);
        },
        showField(field) {
            return this.ready
                && this.$refs.form.showField(field);
        },
        success(data) {
            if (data.message) {
                this.toastr.success(data.message);
            }
        },
    },
};
</script>
