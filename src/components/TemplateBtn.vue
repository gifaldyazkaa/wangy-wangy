<script lang="ts">
import * as Toast from '../funcs/toastify';
import Button from '../shared/Button.vue';
import templates from '../data/templates';

export default {
    name: 'TemplateBtn',
    components: { Button },
    props: ['waifu'],
    data() {
        return {
            waifuName: this.waifu,
            templates,
        };
    },
    methods: {
        setWangy(int) {
            if (!this.waifu) return Toast.error('Tulis nama waifu-mu terlebih dahulu!');
            const template = templates[int];
            const sensitive = template.sensitive;
            let text = template.text;
            if (sensitive)
                this.$swal.fire({
                    title: 'Peringatan',
                    text: 'Template ini mengandung kata kata sensitif! Jika tidak nyaman dengan kata sensitif, silahkan gunakan template lain.',
                    icon: 'warning',
                });
            const nameLower = this.waifu.toLowerCase();
            const nameUpper = this.waifu.toUpperCase();
            const nameUpperFirst = this.waifu.charAt(0).toUpperCase() + this.waifu.slice(1);
            const nameUpperLong = nameUpper + nameUpper.charAt(this.waifu.length - 1).repeat(10);
            text = text.replaceAll(':nameLower:', nameLower);
            text = text.replaceAll(':nameUpper:', nameUpper);
            text = text.replaceAll(':nameUpperFirst:', nameUpperFirst);
            text = text.replaceAll(':nameUpperLong:', nameUpperLong);
            this.$emit('result', text);
        },
    },
};
</script>

<template>
    <section class="template">
        <p>2. Pilih template</p>
        <Button v-for="tpl in templates" @click="setWangy(tpl.num)">{{ tpl.name }}</Button>
    </section>
</template>

<style lang="scss">
@import '../styles/mixin';
.template {
    width: 50%;
    .btn {
        margin-right: 0.5rem;
        margin-top: 0.5rem;
    }
    @include on-mobile() {
        width: 100%;
    }
}
</style>
