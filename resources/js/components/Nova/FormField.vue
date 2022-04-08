<template>
    <DefaultField :field="field" :errors="errors" :show-help-text="showHelpText">
        <template #field>
            <component
                :is="component"
                :name="field.attribute"
                :type="field.type"
                :suggestion-limit="field.suggestionLimit"
                :limit="field.limit"
                :errors="errors"
                :tags="tags"
                :placeholder="field.placeholder"
                :can-be-deselected="field.canBeDeselected"
                v-model="tags"
            ></component>
        </template>
    </DefaultField>
</template>

<script>
import MultiTagsInput from '../Tags/MultiTagsInput';
import SingleTagsInput from '../Tags/SingleTagsInput';
import { FormField, HandlesValidationErrors } from 'laravel-nova';

export default {
    inheritAttrs: false,

    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    data() {
        return {
            tags: [],
        };
    },

    mounted() {
        this.field.value.forEach((value, i) => {
            if (this.value[i].includes('amp;')) {
                this.value[i] = this.value[i].replace('amp;', '');
            }

            this.tags.push(this.value[i]);
        });
    },

    components: {
        MultiTagsInput,
        SingleTagsInput,
    },

    computed: {
        component() {
            return this.field.multiple ? 'MultiTagsInput' : 'SingleTagsInput';
        },
    },

    methods: {
        fill(formData) {
            formData.append(this.field.attribute, this.tags.join('-----'));
        },

        handleChange(value) {
            this.value = value;
        },
    },
};
</script>
