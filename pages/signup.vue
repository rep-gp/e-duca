<template>
    <div class="signup-page">
        <signup-header
            @name-changed="onNameChanged"
            @skin-color-changed="onSkinColorChanged"
            @gender-changed="onGenderChanged"
        />
        <div class="avatar-display">
            <avatar class="avatar" v-bind="avatar"/>
        </div>
        <avatar-color-picker
            @color-changed="onColorChanged"
        />
        <div class="actions">
            <flat-button dark title="Continuar" @click="onContinueClicked" />
        </div>
    </div>
</template>

<script lang="ts">
import Vue from "vue"
import { mapActions } from 'vuex'
import FlatButton from "~/components/common/FlatButton.vue"
import SignupHeader from "~/components/signup/SignupHeader.vue"

export default Vue.extend({
    components: { SignupHeader, FlatButton },
    data: () => ({
        avatar: {},
        name: ''
    }),
    methods: {
        ...mapActions('ui', ['alertInput']),
        ...mapActions('user', ['setUserAvatar', 'setUserData']),
        onNameChanged (name: string) {
            this.name = name
        },
        onSkinColorChanged (skinColor: string) {
            this.avatar = { ...this.avatar, skin: skinColor }
        },
        onGenderChanged (gender: string) {
            this.avatar = { ...this.avatar, gender }
        },
        onColorChanged (color: any) {
            this.avatar = { ...this.avatar, ...color }
        },
        onContinueClicked () {
            if (!this.name) {
                this.alertInput()
                console.log('missing name')
                return
            }
            this.setUserData({ name: this.name })
            this.setUserAvatar(this.avatar)
            this.$router.push('/home')
        }
    }
})
</script>

<style lang="scss" scoped>
.signup-page {
    .avatar-display {
        width: 100%;
        height: 35vh;
        background: #7A2EA3;
        padding-top: 20px;
        & > .avatar {
            display: block;
            margin: 0 auto;
            width: 30vw;
        }
    }
    .actions {
        width: 60%;
        margin: 0 auto;
    }
}
</style>
