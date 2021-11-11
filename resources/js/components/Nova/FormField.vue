<template>
    <default-field :field="field" :errors="errors" :show-help-text="field.helpText">
        <template slot="field">
            <component
                :is="component"
                :name="field.attribute"
                :type="field.type"
                :suggestion-limit="field.suggestionLimit"
                :select-blocks="field.selectBlocks ? field.selectBlocks : false"
                :errors="errors"
                v-model="tags"
            ></component>
        </template>
    </default-field>
</template>

<script>
import MultiTagsInput from '../Tags/MultiTagsInput';
import SingleTagsInput from '../Tags/SingleTagsInput';
import { FormField, HandlesValidationErrors } from 'laravel-nova';

export default {
    inheritAttrs: false,

    mixins: [FormField, HandlesValidationErrors],

    props: ['field'],

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
            return this.field.multiple ? 'multi-tags-input' : 'single-tags-input';
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
