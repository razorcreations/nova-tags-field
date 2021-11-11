<template>
    <div>
        <tags-input
            :tags="tags"
            :type="type"
            :suggestion-limit="suggestionLimit"
            @input="handleInput"
        >
            <div slot-scope="{ tags, removeTag, inputProps, inputEvents, suggestions, insertSuggestion }">
                <div class="tags-input w-full form-control form-input form-input-bordered flex items-center" :class="{ 'border-danger': errors.has(name) }" @click="focusInput">
                    <span v-for="tag in tags" :key="tag" class="tags-input-tag mr-1">
                        <span>{{ tag }}</span>
                        <button
                            type="button"
                            class="tags-input-remove"
                            @click.prevent.stop="removeTag(tag)"
                        >
                            &times;
                        </button>
                    </span>
                    <input
                        ref="input"
                        class="tags-input-text"
                        :placeholder="__('Add tag...')"
                        v-bind="inputProps"
                        v-on="inputEvents"
                    >
                </div>
                <ul v-if="suggestions.length" class="tags-input-suggestions">
                    <li v-for="suggestion in suggestions" :key="suggestion" class="mr-1">
                        <button
                            class="tags-input-tag"
                            @mousedown.prevent
                            @click.prevent="insertSuggestion(suggestion)"
                        >
                            {{ suggestion }}
                        </button>
                    </li>
                </ul>
            </div>
        </tags-input>

        <ul class="tag-blocks" v-if="selectBlocks">
            <li @click="addToTags(tag)" v-for="(tag, index) in selectBlocks" :key="tag+index">
                {{ tag.length > 15 ? tag.substr(0, 15) + '...' : tag }}
            </li>
        </ul>
    </div>
</template>

<script>
import TagsInput from './TagsInput.vue';

export default {
    props: ['name', 'tags', 'type', 'suggestionLimit', 'errors', 'selectBlocks'],

    model: {
        prop: 'tags',
    },

    components: {
        TagsInput,
    },

    methods: {
        focusInput() {
            this.$refs.input.focus();
        },

        handleInput(tags) {
            this.$emit('input', tags);

            // Re-focus the input after a suggestion was inserted
            this.focusInput();
        },

        addToTags(tag) {
            !this.tags.includes(tag) ? this.tags.push(tag) : null;
        }
    },
};
</script>

<style>
.tag-blocks {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: .3rem;
    row-gap: .6rem;
    padding: 0;
    margin-top: 1.2rem;
    height: 7rem;
    overflow-y: scroll;
}

.tag-blocks li {
    list-style-type: none;
    font-size: .8rem;
    background: #4199DE;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    padding: .5rem .2rem;
    cursor: pointer;
}
</style>
