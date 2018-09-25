<template>
    <div class="container">
        <div class="search">
            <input v-model="search"/>
        </div>
        <ul ref="list" class="container">
            <li @click="value = null" :class="[value ? null : 'selected','reset']">
                <font-awesome-icon icon="ban" @click="value = null"/>
            </li>
            <li v-for="(icon,index) in filteredIcons" :key="index" :class="isSelected(icon) ? 'selected' : null" @click="makeSelection(icon)"
                :id="icon.iconName">
                <font-awesome-icon :icon="[icon.prefix,icon.iconName]" @click="makeSelection(icon)"/>
            </li>
        </ul>
    </div>
</template>

<script>
    import Vue from 'vue'
    import {library} from '@fortawesome/fontawesome-svg-core'
    import {fas} from '@fortawesome/free-solid-svg-icons'
    import {fab} from '@fortawesome/free-brands-svg-icons'
    import {FontAwesomeIcon} from '@fortawesome/vue-fontawesome'

    library.add(fas, fab);

    Vue.component('font-awesome-icon', FontAwesomeIcon);

    export default {
        name: "IconPicker",
        props: {
            value: {
                default: null
            }
        },
        data: function () {
            return {
                search: '',
                icons: {...fas, ...fab},
                height: 20,
            }
        },
        mounted: function () {
            try {
                this.$refs.list.getElementsByClassName('selected')[0].scrollIntoView();
                // eslint-disable-next-line
            } catch (e) {
            }


        },
        computed: {
            filteredIcons: function () {
                return Object
                    .keys(this.icons)
                    .filter(key => key !== 'faFontAwesomeLogoFull' && this.icons[key].iconName.includes(this.search))
                    .map(key => this.icons[key]);
            },

        },
        methods: {
            makeSelection: function (icon) {
                this.$emit('input', {
                    type: 'fontawesome',
                    name: icon.iconName
                });
            },
            isSelected: function (icon) {
                return this.value && icon.iconName === this.value.name && this.value.type === 'fontawesome';
            }
        }
    }
</script>
<style scoped>
    .container {
        width: 300px;
        height: 300px;
    }

    ul {
        box-sizing: border-box;
        overflow-y: scroll;
        list-style: none;
        border: 1px solid rgba(0, 0, 0, .2);
        border-radius: 10px;
        padding: 10px;
        margin: 0;
    }

    li {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        padding: 5px;
        margin: 5px;
        width: 20px;
        height: 20px;
        text-align: center;
        border: 1px solid rgba(0, 0, 0, .2);
        border-radius: 50px;
        transition: all .2s ease-in-out;
    }

    li.reset {
        transition: none;
        color: red;
    }

    li.reset:hover, li.reset.selected {
        transform: none;
        background-color: red;
        color: white;
    }

    li:hover {
        background-color: #5a5a5a;
        color: white;
        cursor: pointer;
        transform: scale(2);
        border: 1px solid transparent;
    }

    li.selected {
        transform: scale(2);
        background-color: #1f8aff;
        color: white;
        border: none;
    }

    input {
        margin: 5px;
        width: 100%;
        display: inline-block;
        font-size: 1rem;
        line-height: 1.5;
        color: #495057;
        background-clip: padding-box;
        background-color: transparent;
        border: none;
        transition: border-color .15s ease-in-out, box-shadow .15s ease-in-out;
    }

    input:focus {
        outline: none;
    }

    .search {
        border: 1px solid rgba(0, 0, 0, .2);
        border-radius: 10px;
        margin-bottom: 5px;
    }

</style>