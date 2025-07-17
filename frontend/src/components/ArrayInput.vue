<template>
    <div>
        <div>
            <ul  class="list-group">
                <li v-for="(value, index) in array" :key="index" class="list-group-item">
                    <input :value="array[index]" @input="handleInput($event, index)"  type="text" class="no-bg domain-input" :placeholder="placeholder" />
                    <font-awesome-icon icon="times" class="action remove ms-2 me-3 text-danger" @click="remove(index)" />
                </li>
            </ul>

            <button class="btn btn-normal btn-sm mt-3" @click="addField">{{ $t("addListItem", [ displayName ]) }}</button>
        </div>
    </div>
</template>

<script>
export default {
  inject: ['composeLayout'],
    props: {
        path: {
            type: Array,
            required: true,
        },
        placeholder: {
            type: String,
            default: "",
        },
        displayName: {
            type: String,
            required: true,
        }
    },
    computed: {
        array() {
            console.log(this.doc.getIn(this.path))
            return this.doc.getIn(this.path)?.items || []
        },
        
        doc() {
            return this.composeLayout.yamlDoc.clone()
        },
        
        update() {
            return this.composeLayout.yamlDocChanged
        }
    },
    methods: {
        addField() {
            const doc = this.doc
            if(!doc.getIn(this.path)){
                doc.addIn(this.path, [])
            }
            doc.addIn(this.path, "")
            this.update(doc)
        },
        remove(index) {
            const doc = this.doc
            doc.deleteIn([...this.path, index])
            this.update(doc)
        },
        handleInput(event, index) {
            const doc = this.doc
            doc.setIn([...this.path, index], event.target.value)
            this.update(doc)
        }
    }
};
</script>

<style lang="scss" scoped>
@import "../styles/vars.scss";

.list-group {
    background-color: $dark-bg2;

    li {
        display: flex;
        align-items: center;
        padding: 10px 0 10px 10px;

        .domain-input {
            flex-grow: 1;
            background-color: $dark-bg2;
            border: none;
            color: $dark-font-color;
            outline: none;

            &::placeholder {
                color: #1d2634;
            }
        }
    }
}
</style>
