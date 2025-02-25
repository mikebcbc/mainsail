<template>
    <v-row class="dragable-item my-2 mx-0">
        <v-col class="col-auto pr-0 d-flex py-2">
            <v-icon class="handle">{{ mdiDragVertical }}</v-icon>
        </v-col>
        <v-col class="py-2">
            <settings-row :title="title" :sub-title="subtitle" :dynamic-slot-width="true">
                <v-icon :color="checkboxColor" v-html="checkboxIcon" @click="changeVisibility" />
            </settings-row>
        </v-col>
    </v-row>
</template>

<script lang="ts">
import { Component, Mixins, Prop } from 'vue-property-decorator'
import BaseMixin from '@/components/mixins/base'
import NavigationMixin, { NaviPoint } from '@/components/mixins/navigation'
import SettingsRow from '@/components/settings/SettingsRow.vue'
import draggable from 'vuedraggable'
import { mdiDragVertical, mdiCheckboxMarked, mdiCheckboxBlankOutline } from '@mdi/js'

@Component({
    components: { SettingsRow, draggable },
})
export default class SettingsNavigationTab extends Mixins(NavigationMixin, BaseMixin) {
    mdiDragVertical = mdiDragVertical

    @Prop({ type: Object, required: true }) naviPoint!: NaviPoint

    get title() {
        return this.naviPoint.title
    }

    get subtitle() {
        if (this.naviPoint.type === 'link') return `URL: ${this.naviPoint.href ?? 'Unknown'}`

        return undefined
    }

    get checkboxColor() {
        if (this.naviPoint.visible) return 'primary'

        return 'grey lighten-1'
    }

    get checkboxIcon() {
        if (this.naviPoint.visible) return mdiCheckboxMarked

        return mdiCheckboxBlankOutline
    }

    changeVisibility() {
        this.$store.dispatch('gui/navigation/changeVisibility', this.naviPoint)
    }
}
</script>

<style scoped>
.dragable-item {
    background-color: #282828;
}
</style>
