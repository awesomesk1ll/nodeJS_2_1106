<template>
    <div class="products">
        <item v-for="item of filtered" :key="item._id" :item="item"/>
        <item :type="'temp'" @createnew="addNewCatalogItem"/>
    </div>
</template>

<script>
import item from '../components/Item.vue'

export default {
    components: { item },
    // components: { catalogItem },
    data() {
        return {
            items: [],
            filtered: [],
            url: '/api/catalog'
        }
    },
    mounted() {
        this.$parent.get(this.url).then(d => {
            this.items = d;
            this.filtered = d;
        })
    },
    methods: {
        filter(str) {
            let reg = new RegExp(str, 'i');
            this.filtered = this.items.filter(el => reg.test(el.name));
        },
        addNewCatalogItem(item) { //{name, price}
            let newItem = JSON.parse(JSON.stringify(item));
            this.$parent.post('/api/catalog/', newItem)
                .then(res => {
                    if (res.id) {
                        this.items.push(Object.assign({}, newItem, { id_product: res.id }));
                    }
                });
        }
    }
}
</script>

<style>

</style>