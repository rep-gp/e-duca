<template>
    <div :class="['userbar', { '--expanded': userbarExpanded }]">
        <arrow-icon class="userbar-arrow" @click="closeUserbar" />

        <div class="search">
            <input-field
                label="Procurar amigos"
                :value="search"
                bordered blue icon
                @input="onInput"
            />

            <div class="add"> + </div>
        </div>


        <toggle class="toggle" :options="['Amigos', 'Grupo']" @toggle="onToggle" />

        <div class="friends">
            <friends-row
                v-for="(friend, index) in filteredFriends"
                :key="index + friend.name"
                v-bind="friend"
            />
            <div v-if="!filteredFriends.length" class="no-friends"> Nenhum amigo encontrado :( </div>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex'

import FriendsRow from '@/components/friends/FriendsRow'
import Toggle from '@/components/common/Toggle'
import InputField from '@/components/common/InputField'

import ArrowIcon from '@/static/icons/arrow-left.svg'

export default {
    components: {
        Toggle,
        InputField,
        FriendsRow,
        ArrowIcon
    },

    data () {
        return {
            search: ''
        }
    },

    computed: {
        ...mapState('ui', ['userbarExpanded']),
        ...mapState('user', ['userFriends']),
        filteredFriends () {
            return this.userFriends.filter(u => JSON.stringify(u).match(new RegExp(this.search, 'i')))
        }
    },

    methods: {
        onToggle (selection) {
            if (selection === 'Grupo') {
                this.showWipModal('Ver grupo')
            }
        },
        closeUserbar () {
            this.$store.dispatch('ui/setUserbarExpand', false)
        },
        onInput (param) {
            this.search = param
        },
        onChange (param) {
            console.log(param, 'change')
        }
    }
}
</script>

<style lang="scss" scoped>
.userbar {
    right: 0;
    width: 0;
    background: $white;
    height: 100vh;
    padding: 6vw;
    position: fixed;
    z-index: 999;
    box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.349);
    transition: $ease-out;
    visibility: hidden;
    opacity: 0;

    &.--expanded {
        opacity: 1;
        visibility: visible;
        transition: $ease-in;
        width: $userbar-width;

        .friends {
            transition: .4s;
            opacity: 1;
        }
    }

    &-arrow {
        width: 5vw;
        margin-bottom: 5vw;
        fill: $blue-main;
        transform: rotate(180deg);
    }

    .search {
        display: flex;
        align-items: center;

        .add {
            border: 2px solid $blue-main;
            border-radius: 6px;
            padding: .5vw 2vw;
            height: 100%;
            margin-left: 2vw;
        }
    }

    .toggle {
        margin: 4vh 0 2vh;
    }
    
    .friends {
        opacity: 0;
        transition: $ease-out;
    }
    .no-friends {
        padding: 6vw;
        text-align: center;
    }
}
</style>