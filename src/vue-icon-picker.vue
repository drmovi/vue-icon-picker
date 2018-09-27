<template>
    <div class="container">
        <div class="search" v-click-outside="hideColorPicker">
            <div @click="toggleColorPicker($event)">
                <font-awesome-icon v-if="icon" :icon="icon.iconName" class="icon-preview" :style="{color:color.hex}"/>
            </div>
            <input v-model="search" @keyup="scrollToSelected"/>
            <div v-show="colorPickerPosition" :style="colorPickerPosition">
                <chrome-picker v-model="color"/>
                <button type="button" class="btn" @click.prevent="color = {}">Clear Color</button>
            </div>

        </div>
        <ul ref="list" :style="{'height':height}">
            <li @click="resetValue" :class="[value ? null : 'selected','reset']">
                <font-awesome-icon icon="ban" @click="resetValue"/>
            </li>
            <li v-for="(filteredIcon,index) in filteredIcons" :key="index" :class="isSelected(filteredIcon) ? 'selected' : null"
                @click="icon = filteredIcon">
                <font-awesome-icon :icon="[filteredIcon.prefix,filteredIcon.iconName]"/>
            </li>
        </ul>
    </div>
</template>


<script>
    import {library} from '@fortawesome/fontawesome-svg-core'
    import {fas} from '@fortawesome/free-solid-svg-icons'
    import {fab} from '@fortawesome/free-brands-svg-icons'
    import {FontAwesomeIcon} from '@fortawesome/vue-fontawesome'
    import {Chrome} from 'vue-color'
    import ClickOutside from 'vue-click-outside'


    library.add(fas, fab);
    export default {
        name: "VueIconPicker",
        components: {
            FontAwesomeIcon,
            'chrome-picker': Chrome
        },
        directives: {
            ClickOutside
        },
        props: {
            value: null,
            height: {
                type: String,
                default: '300px'
            }
        },
        data: function () {
            return {
                search: '',
                icons: {...fas, ...fab},
                icon: null,
                color: {},
                colorPickerPosition: null,
                popupItem: null
            }
        },
        mounted: function () {
            this.popupItem = this.$el;
            try {
                this.icon = this.icons[Object.keys(this.icons).find(icon => this.icons[icon].iconName === this.value.name)];
                this.color = {hex: this.value.color};
                this.scrollToSelected();
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
            scrollToSelected: function () {
                try {
                    this.$refs.list.getElementsByClassName('selected')[0].scrollIntoView();
                    // eslint-disable-next-line
                } catch (e) {
                }
            },
            isSelected: function (icon) {
                return this.value && icon.iconName === this.value.name && this.value.type === 'fontawesome';
            },
            resetValue: function () {
                this.icon = null;
                this.color = {};
                this.colorPickerPosition = null;
            },
            toggleColorPicker: function (e) {
                this.colorPickerPosition = this.colorPickerPosition ? null : {
                    position: 'absolute',
                    zIndex: 9999,
                    top: `${e.pageY}px`,
                    left: `${e.pageX}px`,

                }
            },
            hideColorPicker: function () {
                this.colorPickerPosition = null;
            }
        },
        watch: {
            icon: function (value) {
                this.$emit('input', value ? {...this.value, name: value.iconName, type: 'fontawesome'} : null);
            },
            color: function (value) {
                let val = this.value;
                if (value.hex)
                    val.color = value.hex;
                else
                    delete val.color;
                this.$emit('input', val)
            }
        }
    }

</script>
<style scoped>
    .container {
        position: relative;
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
        display: block;
        width: calc(100% - 10px);
        border: 1px solid rgba(0, 0, 0, .2);
        padding: 5px;
        border-radius: 10px;
        margin: 5px 0;
        font-size: 1rem;
        line-height: 1.5;
        color: #495057;
        background-clip: padding-box;
        background-color: transparent;
        transition: border-color .15s ease-in-out, box-shadow .15s ease-in-out;
    }

    input:focus {
        outline: none;
    }

    .search {
        margin-bottom: 5px;
        position: relative;
    }

    .icon-preview {
        cursor: pointer;
        border: 1px solid rgba(0, 0, 0, .2);
        padding: 10px;
        width: 40px;
        height: 40px;
        border-radius: 10px;
    }

    .btn {
        color: white;
        width: 100%;
        vertical-align: middle;
        user-select: none;
        border: 1px solid transparent;
        padding: 5px;
        outline: none;
        box-shadow: 0 0 2px rgba(0, 0, 0, .3), 0 4px 8px rgba(0, 0, 0, .3);
        background-color: #bd2130;
        cursor: pointer;
    }

    .btn:active {
        color: #fff;
        background-color: #900b18;
    }
</style>